# API Patterns: Endpoints That Don't Embarrass You

## Endpoint Structure

### Consistent response format everywhere

```typescript
// Success
{ "data": { ... }, "error": null }

// Error  
{ "data": null, "error": { "code": "VALIDATION", "message": "Start date required" } }

// List
{ "data": [...], "pagination": { "total": 42, "page": 1, "limit": 20 }, "error": null }
```

Never mix formats. Never return a bare object for one endpoint and wrapped for another.

### Route organization (Next.js App Router)

```
app/api/
  machines/
    route.ts          → GET (list), POST (create)
    [id]/
      route.ts        → GET (one), PATCH (update), DELETE (delete)
  bookings/
    route.ts          → GET (list), POST (create)
    [id]/
      route.ts        → GET, PATCH
      return/
        route.ts      → POST (custom action)
  customers/
    route.ts          → GET, POST
  export/
    invoices/
      route.ts        → GET (download CSV)
```

### Route handler pattern

```typescript
// app/api/bookings/route.ts
import { NextResponse } from "next/server";
import { getSession } from "@/lib/auth";
import { db } from "@/lib/database";
import { AppError } from "@/lib/errors";
import { CreateBookingSchema } from "./validation";

export async function GET(req: Request) {
  try {
    const session = await getSession();
    if (!session) return unauthorized();

    const { searchParams } = new URL(req.url);
    const status = searchParams.get("status");
    const page = parseInt(searchParams.get("page") ?? "1");

    const where = {
      ...(status && { status }),
      deletedAt: null,
    };

    const [bookings, total] = await Promise.all([
      db.booking.findMany({
        where,
        include: { machine: true, customer: true },
        orderBy: { createdAt: "desc" },
        take: 20,
        skip: (page - 1) * 20,
      }),
      db.booking.count({ where }),
    ]);

    return NextResponse.json({
      data: bookings,
      pagination: { total, page, limit: 20 },
      error: null,
    });
  } catch (err) {
    return handleError(err);
  }
}

export async function POST(req: Request) {
  try {
    const session = await getSession();
    if (!session) return unauthorized();

    const body = await req.json();
    const parsed = CreateBookingSchema.safeParse(body);
    if (!parsed.success) {
      return validationError(parsed.error.issues[0].message);
    }

    const booking = await createBooking(parsed.data);
    return NextResponse.json({ data: booking, error: null }, { status: 201 });
  } catch (err) {
    return handleError(err);
  }
}

// Reusable error helpers
function unauthorized() {
  return NextResponse.json(
    { data: null, error: { code: "UNAUTHORIZED", message: "Login required" } },
    { status: 401 }
  );
}

function validationError(message: string) {
  return NextResponse.json(
    { data: null, error: { code: "VALIDATION", message } },
    { status: 400 }
  );
}

function handleError(err: unknown) {
  if (err instanceof AppError) {
    return NextResponse.json(
      { data: null, error: { code: err.code, message: err.message } },
      { status: err.status }
    );
  }
  console.error("Unhandled error:", err);
  return NextResponse.json(
    { data: null, error: { code: "INTERNAL", message: "Something went wrong" } },
    { status: 500 }
  );
}
```

## Validation at the Edge

Validate EVERYTHING that comes from outside. Then internal code can trust the data.

```typescript
import { z } from "zod";

export const CreateBookingSchema = z.object({
  machineId: z.string().min(1, "Machine required"),
  customerId: z.string().min(1, "Customer required"),
  startDate: z.coerce.date({ required_error: "Start date required" }),
  endDate: z.coerce.date({ required_error: "End date required" }),
}).refine(
  (d) => d.startDate < d.endDate,
  { message: "Start must be before end", path: ["endDate"] }
).refine(
  (d) => d.startDate >= new Date(new Date().setHours(0,0,0,0)),
  { message: "Cannot book in the past", path: ["startDate"] }
);
```

## Auth Middleware

```typescript
// lib/auth.ts
import { getServerSession } from "next-auth";
import { authOptions } from "@/app/api/auth/[...nextauth]/options";

export async function getSession() {
  return getServerSession(authOptions);
}

export async function requireSession() {
  const session = await getSession();
  if (!session?.user) throw new AppError("Authentication required", 401, "UNAUTHORIZED");
  return session;
}
```

## Rate Limiting (Simple)

```typescript
// In-memory rate limiter (fine for single-server)
const rateLimits = new Map<string, { count: number; resetAt: number }>();

export function rateLimit(key: string, maxRequests = 60, windowMs = 60000) {
  const now = Date.now();
  const entry = rateLimits.get(key);

  if (!entry || now > entry.resetAt) {
    rateLimits.set(key, { count: 1, resetAt: now + windowMs });
    return;
  }

  if (entry.count >= maxRequests) {
    throw new AppError("Too many requests", 429, "RATE_LIMITED");
  }

  entry.count++;
}
```

## Idempotency

POST endpoints should be safe to retry:

```typescript
export async function POST(req: Request) {
  const body = await req.json();
  const idempotencyKey = req.headers.get("Idempotency-Key");

  if (idempotencyKey) {
    const existing = await db.booking.findFirst({
      where: { idempotencyKey },
    });
    if (existing) return NextResponse.json({ data: existing, error: null });
  }

  const booking = await createBooking({ ...body, idempotencyKey });
  return NextResponse.json({ data: booking, error: null }, { status: 201 });
}
```
