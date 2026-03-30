# Code Structure: Where Things Live

## The 200-Line Rule

No file exceeds 200 lines. This is not arbitrary. 200 lines is roughly what fits in a developer's working memory. Beyond that, you're scrolling and forgetting.

When a file approaches 200 lines:
1. Identify the distinct responsibilities in the file
2. Extract each into its own file
3. The original file becomes a thin orchestrator that imports and coordinates

```
# BAD: one 800-line file
pages/bookings.tsx  (routing + form + validation + API calls + UI + state)

# GOOD: decomposed by responsibility
pages/bookings/index.tsx        (route, ~30 lines)
pages/bookings/BookingForm.tsx  (form UI, ~80 lines)
pages/bookings/useBooking.ts    (data fetching + state, ~60 lines)
pages/bookings/validation.ts    (form validation, ~40 lines)
lib/api/bookings.ts             (API client functions, ~50 lines)
```

## Naming Conventions

### Files
- React components: `PascalCase.tsx` (BookingForm.tsx)
- Everything else: `camelCase.ts` or `kebab-case.ts` (pick one, be consistent)
- Test files: same name + `.test.ts` (BookingForm.test.tsx)
- Type files: same name + `.types.ts` when types are shared across files

### Functions
- Actions: verb + noun (`createBooking`, `validateInput`, `formatCurrency`)
- Booleans: `is/has/should` prefix (`isAvailable`, `hasExpired`, `shouldRefresh`)
- Handlers: `handle` + event (`handleSubmit`, `handleDateChange`)
- Computed values: descriptive noun (`totalCost`, `availableMachines`)

### Variables
- Avoid single letters except in tiny scopes (`i` in a 3-line loop is fine)
- Avoid abbreviations (`btn` → `button`, `mgr` → `manager`, `curr` → `current`)
- Collections are plural (`bookings`, `machines`), single items are singular (`booking`)

### Constants
```typescript
// SCREAMING_SNAKE for true constants
const MAX_BOOKING_DAYS = 90;
const API_TIMEOUT_MS = 5000;
const ACCOUNTING_API_URL = "https://api.example.com/v3";
```

## Function Design

### One function, one job

```typescript
// BAD: function does 4 things
async function handleBookingSubmit(formData) {
  // validates
  // checks availability
  // creates booking
  // sends email notification
}

// GOOD: each function does one thing
async function handleBookingSubmit(formData) {
  const validated = validateBookingInput(formData);
  await assertMachineAvailable(validated.machineId, validated.startDate, validated.endDate);
  const booking = await createBooking(validated);
  await notifyOwner(booking);
  return booking;
}
```

The orchestrating function reads like a checklist. Each step is a function you can test independently.

### Function length

Target: under 20 lines. Acceptable: up to 40 lines for complex logic. Unacceptable: 100+ lines.

If a function is long, it's doing too much. Extract helper functions.

### Parameters

Maximum 3 parameters. Beyond that, use an object:

```typescript
// BAD: what's the 4th boolean again?
createBooking(machineId, customerId, startDate, endDate, true, false);

// GOOD: named parameters via object
createBooking({
  machineId,
  customerId,
  startDate,
  endDate,
  autoConfirm: true,
  sendNotification: false,
});
```

## Module Organization

### Group by feature, not by type

```
# BAD: grouped by type (you open 5 folders to understand one feature)
components/
  BookingForm.tsx
  MachineCard.tsx
  CustomerList.tsx
services/
  bookingService.ts
  machineService.ts
hooks/
  useBooking.ts
  useMachines.ts

# GOOD: grouped by feature (everything about bookings is together)
features/
  bookings/
    BookingForm.tsx
    useBooking.ts
    bookingService.ts
    validation.ts
    index.ts
  machines/
    MachineCard.tsx
    useMachines.ts
    machineService.ts
    index.ts
```

Feature grouping means: to understand bookings, you look in ONE folder.

### The index.ts pattern

Each feature folder has an `index.ts` that exports only the public API:

```typescript
// features/bookings/index.ts
export { BookingForm } from "./BookingForm";
export { useBookings, useBooking } from "./useBooking";
export type { Booking, CreateBookingInput } from "./types";
// Internal implementation details are NOT exported
```

## Import Organization

```typescript
// 1. External packages
import { useState, useEffect } from "react";
import { useRouter } from "next/navigation";

// 2. Internal absolute imports
import { AppError } from "@/lib/errors";
import { db } from "@/lib/database";

// 3. Relative imports (same feature)
import { BookingForm } from "./BookingForm";
import { validateBookingInput } from "./validation";

// 4. Types (if separate)
import type { Booking } from "./types";
```

## Duplication vs. Abstraction

### The Rule of Three

Don't abstract on the first duplication. Don't abstract on the second. Abstract on the THIRD.

Why: the first two instances often diverge as requirements evolve. Premature abstraction creates a shared module that serves neither case well.

```typescript
// First time: just write it
function formatBookingDate(date: Date) {
  return date.toLocaleDateString("sv-SE");
}

// Second time: copy it (yes, really)
function formatInvoiceDate(date: Date) {
  return date.toLocaleDateString("sv-SE");
}

// Third time: NOW abstract
function formatDate(date: Date, locale = "sv-SE") {
  return date.toLocaleDateString(locale);
}
```

### When abstraction IS right

- Three or more identical implementations
- A bug fix would need to be applied in multiple places
- The shared logic is genuinely the SAME concept (not just happens to look similar today)

## State Management

### Start simple, add complexity only when needed

```
Level 0: Local state (useState) — for UI state within one component
Level 1: Lifted state — for state shared between parent and child
Level 2: Context — for state shared across many components (auth, theme)
Level 3: URL state — for state that should survive page refresh (filters, pagination)
Level 4: Server state (React Query/SWR) — for data from APIs
Level 5: Global store (Zustand/Redux) — almost never needed for small apps
```

AI defaults to Level 5 for everything. Most apps need Level 0-4 only.
