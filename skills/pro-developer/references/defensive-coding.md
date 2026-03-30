# Defensive Coding: When Things Go Wrong

## The Principle

AI writes code for the happy path. Senior developers write code for the sad path FIRST, then add the happy path.

Every function has three possible outcomes:
1. Success (the happy path)
2. Expected failure (invalid input, not found, auth failure)
3. Unexpected failure (network timeout, disk full, OOM)

Your code must handle all three. Not with `console.error`. With actual user-visible behavior.

## Error Handling Strategy

### Pick ONE strategy and use it everywhere

**Option A: Throw + Catch at boundaries (recommended for most apps)**
```typescript
// Deep in the code: throw when something's wrong
async function getBooking(id: string) {
  if (!id) throw new AppError("Booking ID is required", 400);
  const booking = await db.booking.findUnique({ where: { id } });
  if (!booking) throw new AppError("Booking not found", 404);
  return booking;
}

// At the API boundary: catch and format
app.get("/api/bookings/:id", async (req, res) => {
  try {
    const booking = await getBooking(req.params.id);
    res.json({ data: booking, error: null });
  } catch (err) {
    if (err instanceof AppError) {
      res.status(err.status).json({ data: null, error: { code: err.code, message: err.message } });
    } else {
      logger.error("Unexpected error", err);
      res.status(500).json({ data: null, error: { code: "INTERNAL", message: "Something went wrong" } });
    }
  }
});
```

**Option B: Result types (for complex business logic)**
```typescript
type Result<T> = { ok: true; data: T } | { ok: false; error: string };

async function getBooking(id: string): Promise<Result<Booking>> {
  if (!id) return { ok: false, error: "Booking ID is required" };
  const booking = await db.booking.findUnique({ where: { id } });
  if (!booking) return { ok: false, error: "Booking not found" };
  return { ok: true, data: booking };
}
```

**NEVER: mixed strategies**
```typescript
// Some functions throw, some return null, some return { error }
// This is AI's default and it's a maintenance nightmare
```

### The AppError Pattern

```typescript
class AppError extends Error {
  constructor(
    message: string,
    public status: number = 500,
    public code: string = "UNKNOWN"
  ) {
    super(message);
    this.name = "AppError";
  }
}

// Usage:
throw new AppError("Machine already booked for these dates", 409, "BOOKING_CONFLICT");
throw new AppError("Customer not found", 404, "NOT_FOUND");
throw new AppError("Invalid date range: start must be before end", 400, "VALIDATION");
```

## Input Validation

### Validate at the boundary, trust internally

```typescript
// API route: validate everything from the outside world
app.post("/api/bookings", async (req, res) => {
  const { machineId, customerId, startDate, endDate } = req.body;

  // Validate presence
  if (!machineId) throw new AppError("Machine ID required", 400);
  if (!customerId) throw new AppError("Customer ID required", 400);
  if (!startDate || !endDate) throw new AppError("Start and end date required", 400);

  // Validate types
  const start = new Date(startDate);
  const end = new Date(endDate);
  if (isNaN(start.getTime())) throw new AppError("Invalid start date", 400);
  if (isNaN(end.getTime())) throw new AppError("Invalid end date", 400);

  // Validate business rules
  if (start >= end) throw new AppError("Start date must be before end date", 400);
  if (start < new Date()) throw new AppError("Cannot book in the past", 400);

  // Now call internal function with trusted, validated data
  const booking = await createBooking({ machineId, customerId, start, end });
  res.status(201).json({ data: booking, error: null });
});
```

### Use Zod or similar for complex validation

```typescript
import { z } from "zod";

const CreateBookingSchema = z.object({
  machineId: z.string().min(1, "Machine ID required"),
  customerId: z.string().min(1, "Customer ID required"),
  startDate: z.string().datetime("Invalid start date"),
  endDate: z.string().datetime("Invalid end date"),
}).refine(
  (data) => new Date(data.startDate) < new Date(data.endDate),
  { message: "Start date must be before end date" }
);

// Usage:
const parsed = CreateBookingSchema.safeParse(req.body);
if (!parsed.success) {
  throw new AppError(parsed.error.issues[0].message, 400, "VALIDATION");
}
```

## Null/Undefined Handling

### The #1 runtime error in JavaScript/TypeScript

AI code constantly does this:
```typescript
// CRASH: if booking is null, accessing .machine throws
const name = booking.machine.name;
```

Senior dev:
```typescript
// Safe: optional chaining + fallback
const name = booking?.machine?.name ?? "Unknown";

// Or: explicit check when null means "something is actually wrong"
if (!booking) throw new AppError("Booking not found", 404);
if (!booking.machine) {
  logger.error(`Booking ${booking.id} has no machine linked`);
  throw new AppError("Booking data is corrupted", 500);
}
```

### Rule: every `.` access on data from outside your function is a potential crash

External data includes: database results, API responses, URL params, form data, localStorage, file reads.

## Async Error Handling

### Every await needs error consideration

```typescript
// AI-default: no error handling on await
const user = await db.user.findUnique({ where: { id } });
const orders = await db.order.findMany({ where: { userId: id } });

// What if db is down? What if connection times out?
// Both lines crash with unhandled promise rejection.

// Senior dev: handle at the appropriate level
try {
  const user = await db.user.findUnique({ where: { id } });
  if (!user) throw new AppError("User not found", 404);

  const orders = await db.order.findMany({ where: { userId: id } });
  return { user, orders };
} catch (err) {
  if (err instanceof AppError) throw err; // Re-throw known errors
  logger.error("Database error in getUser", err);
  throw new AppError("Failed to load user data", 503);
}
```

### External API calls: ALWAYS have timeouts

```typescript
// AI-default: no timeout, waits forever
const response = await fetch("https://api.example.com/invoices");

// Senior dev: explicit timeout
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000); // 5s timeout

try {
  const response = await fetch("https://api.example.com/invoices", {
    signal: controller.signal,
  });
  if (!response.ok) {
    throw new AppError(`External API error: ${response.status}`, 502);
  }
  return await response.json();
} catch (err) {
  if (err.name === "AbortError") {
    throw new AppError("External API timed out", 504);
  }
  throw err;
} finally {
  clearTimeout(timeout);
}
```

## Race Conditions

### Double-submit prevention

```typescript
// AI ignores this entirely. Users double-click. Always.

// Backend: use idempotency keys or database constraints
const existing = await db.booking.findFirst({
  where: { machineId, startDate, endDate, status: { not: "cancelled" } },
});
if (existing) throw new AppError("This booking already exists", 409);

// Frontend: disable button after first click
const [isSubmitting, setIsSubmitting] = useState(false);
async function handleSubmit() {
  if (isSubmitting) return;
  setIsSubmitting(true);
  try {
    await createBooking(formData);
  } finally {
    setIsSubmitting(false);
  }
}
```

### Booking conflicts (concurrent access)

```typescript
// AI: check-then-insert (race condition!)
const conflicts = await db.booking.findMany({ where: { machineId, ... } });
if (conflicts.length === 0) {
  await db.booking.create({ ... }); // Another request could insert between check and create
}

// Senior dev: use database-level constraints
// Option 1: Unique constraint or exclusion constraint (Postgres)
// Option 2: Transaction with SELECT FOR UPDATE
// Option 3: Application-level lock (acceptable for single-server SQLite)
await db.$transaction(async (tx) => {
  const conflicts = await tx.booking.findMany({
    where: { machineId, status: { not: "cancelled" },
      OR: [
        { startDate: { lte: endDate }, endDate: { gte: startDate } },
      ],
    },
  });
  if (conflicts.length > 0) {
    throw new AppError("Machine is already booked for these dates", 409);
  }
  return tx.booking.create({ data: { ... } });
});
```

## The Defensive Coding Checklist

Before presenting any code:

- [ ] Every database query handles "not found" (null result)
- [ ] Every external API call has a timeout
- [ ] Every form submit is protected against double-click
- [ ] Every user-facing error has a clear message (not "Error" or "Something went wrong")
- [ ] No `catch (e) { console.error(e) }` that swallows errors silently
- [ ] No `.property` access on potentially null values without check
- [ ] No TODOs, no placeholders, no "implement later"
- [ ] Input validation happens at the boundary (API route), not deep inside
