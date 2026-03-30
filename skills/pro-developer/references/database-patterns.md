# Database Patterns: Queries That Don't Betray You

## The N+1 Problem

The most common performance bug in AI-generated code. Invisible at 10 records, catastrophic at 1000.

```typescript
// BAD: N+1 (1 query for bookings + 1 per booking for machine)
const bookings = await db.booking.findMany();
for (const booking of bookings) {
  booking.machine = await db.machine.findUnique({ where: { id: booking.machineId } });
}
// 100 bookings = 101 queries

// GOOD: eager load (1 query total, or 2 with Prisma)
const bookings = await db.booking.findMany({
  include: { machine: true, customer: true },
});
// 100 bookings = 2 queries (bookings + machines via IN clause)
```

**Rule: if you have a loop with a database query inside, you have an N+1 bug.**

## Query Patterns

### Select only what you need
```typescript
// BAD: select everything
const machines = await db.machine.findMany();

// GOOD: select only displayed fields
const machines = await db.machine.findMany({
  select: { id: true, name: true, status: true, dailyRate: true },
});
```

### Pagination from day one
```typescript
// Every list endpoint should be paginated
const bookings = await db.booking.findMany({
  take: 20,
  skip: (page - 1) * 20,
  orderBy: { createdAt: "desc" },
});

// Cursor-based for infinite scroll (better performance)
const bookings = await db.booking.findMany({
  take: 20,
  cursor: lastId ? { id: lastId } : undefined,
  skip: lastId ? 1 : 0,
  orderBy: { createdAt: "desc" },
});
```

### Filter injection prevention
```typescript
// Prisma and most ORMs parameterize automatically.
// But with raw SQL, ALWAYS use parameterized queries:

// BAD: string concatenation (SQL injection)
const result = await db.$queryRaw`SELECT * FROM machines WHERE name = '${name}'`;

// GOOD: parameterized
const result = await db.$queryRaw`SELECT * FROM machines WHERE name = ${name}`;
```

## Transactions

### When you need them
Any operation that modifies multiple records that must succeed or fail together:

```typescript
// Creating a booking must: check availability AND create the record
// If these aren't atomic, two people could book the same machine
await db.$transaction(async (tx) => {
  const conflicts = await tx.booking.findMany({
    where: {
      machineId,
      status: { not: "cancelled" },
      startDate: { lte: endDate },
      endDate: { gte: startDate },
    },
  });

  if (conflicts.length > 0) {
    throw new AppError("Machine already booked for these dates", 409);
  }

  return tx.booking.create({
    data: { machineId, customerId, startDate, endDate, status: "reserved", dailyRate },
  });
});
```

### When you don't need them
Single-record operations. A single INSERT, UPDATE, or DELETE is already atomic.

## Migrations

### Rules
1. Every schema change gets a migration. Never modify the database directly.
2. Every migration must be reversible (write the down migration).
3. Never rename or drop a column in production without a deprecation period.
4. Test migrations on a copy of production data before running in production.

### The expand-contract pattern (for breaking changes)
```
Step 1: ADD new column (expand). Deploy code that writes to both.
Step 2: Backfill old data into new column.
Step 3: Deploy code that reads from new column.
Step 4: DROP old column (contract).
```

Never combine steps 1 and 4 in one deploy. You need rollback ability between each step.

## Indexing

### What to index
- Every foreign key (machineId, customerId on bookings)
- Columns in WHERE clauses of frequent queries
- Columns in ORDER BY of frequent queries
- Composite indexes for compound WHERE clauses

```prisma
model Booking {
  id          String   @id @default(cuid())
  machineId   String
  customerId  String
  startDate   DateTime
  endDate     DateTime
  status      String
  
  machine     Machine  @relation(fields: [machineId], references: [id])
  customer    Customer @relation(fields: [customerId], references: [id])
  
  @@index([machineId, status])      // "bookings for machine X that are active"
  @@index([customerId])              // "all bookings for customer Y"
  @@index([status, endDate])         // "active bookings ending soon"
}
```

### What NOT to index
- Columns with very low cardinality (boolean `isActive` with 50/50 split)
- Tables with fewer than 1000 rows (full scan is faster than index lookup)
- Columns that are rarely queried

## Soft Delete vs Hard Delete

For data that matters (bookings, customers, invoices): soft delete.

```prisma
model Booking {
  // ...
  deletedAt DateTime?  // null = active, date = soft-deleted
}

// Query: always filter out deleted
const bookings = await db.booking.findMany({
  where: { deletedAt: null },
});

// "Delete": set timestamp
await db.booking.update({
  where: { id },
  data: { deletedAt: new Date() },
});
```

For ephemeral data (sessions, temp tokens, job queue): hard delete is fine.
