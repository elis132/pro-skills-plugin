# React Patterns: What Senior Devs Actually Do

## Component Design

### Props: minimal and typed

```tsx
// BAD: kitchen-sink props
interface CardProps {
  title: string;
  subtitle?: string;
  image?: string;
  onClick?: () => void;
  onHover?: () => void;
  variant?: "primary" | "secondary" | "ghost" | "outline" | "danger";
  size?: "xs" | "sm" | "md" | "lg" | "xl";
  rounded?: boolean;
  shadow?: boolean;
  border?: boolean;
  className?: string;
  // ... 15 more props
}

// GOOD: focused component with clear purpose
interface MachineCardProps {
  machine: Machine;
  onSelect: (id: string) => void;
}
```

A component with 15+ props is doing too much. Split it.

### Composition over configuration

```tsx
// BAD: one component does everything via props
<Card variant="booking" showActions showDates showCustomer size="lg" />

// GOOD: compose from smaller pieces
<Card>
  <Card.Header>
    <MachineInfo machine={booking.machine} />
  </Card.Header>
  <Card.Body>
    <DateRange start={booking.startDate} end={booking.endDate} />
    <CustomerBadge customer={booking.customer} />
  </Card.Body>
  <Card.Footer>
    <ReturnButton bookingId={booking.id} />
  </Card.Footer>
</Card>
```

### Component size

A component should fit on one screen (~50-80 lines). If it doesn't, extract sub-components.

The extraction rule: if a chunk of JSX has its own state or its own logic, it's a component.

## State Management

### useState: the right default

```tsx
// For simple UI state: useState is perfect
const [isOpen, setIsOpen] = useState(false);
const [selectedDate, setSelectedDate] = useState<Date | null>(null);
```

### When NOT to useState

**Derived state: compute, don't store**
```tsx
// BAD: storing derived state
const [machines, setMachines] = useState<Machine[]>([]);
const [availableMachines, setAvailableMachines] = useState<Machine[]>([]);

// Every time machines changes, you must remember to update availableMachines

// GOOD: derive it
const [machines, setMachines] = useState<Machine[]>([]);
const availableMachines = machines.filter(m => m.status === "available");
// Expensive? Use useMemo:
const availableMachines = useMemo(
  () => machines.filter(m => m.status === "available"),
  [machines]
);
```

**URL state: use the URL**
```tsx
// BAD: filter state in useState (lost on refresh)
const [statusFilter, setStatusFilter] = useState("active");

// GOOD: filter state in URL (survives refresh, shareable)
const searchParams = useSearchParams();
const statusFilter = searchParams.get("status") ?? "active";
```

**Server state: use React Query / SWR**
```tsx
// BAD: manual loading/error/data state
const [bookings, setBookings] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  setLoading(true);
  fetch("/api/bookings")
    .then(r => r.json())
    .then(data => setBookings(data))
    .catch(err => setError(err))
    .finally(() => setLoading(false));
}, []);

// GOOD: let the library handle it
const { data: bookings, isLoading, error } = useQuery({
  queryKey: ["bookings"],
  queryFn: () => fetch("/api/bookings").then(r => r.json()),
});
```

## useEffect: Use Less of It

### Most useEffects are wrong

```tsx
// BAD: useEffect for derived computation
useEffect(() => {
  setTotal(items.reduce((sum, item) => sum + item.price, 0));
}, [items]);
// This causes an extra render. Just compute it directly.

// BAD: useEffect for event response
useEffect(() => {
  if (isSubmitted) {
    navigate("/success");
  }
}, [isSubmitted]);
// This should be in the submit handler, not an effect.

// GOOD: useEffect for synchronization with external systems
useEffect(() => {
  const ws = new WebSocket(url);
  ws.onmessage = (e) => setData(JSON.parse(e.data));
  return () => ws.close();
}, [url]);
```

**Rule: if you can do it in an event handler, don't use useEffect.**

## Loading and Error States

### Every async operation needs three states

```tsx
function BookingList() {
  const { data, isLoading, error } = useBookings();

  if (isLoading) return <LoadingSkeleton />;
  if (error) return <ErrorMessage message="Failed to load bookings" retry={refetch} />;
  if (data.length === 0) return <EmptyState message="No bookings yet" action="Create your first booking" />;

  return <BookingTable bookings={data} />;
}
```

AI typically handles `data` and ignores `loading` and `error`. A shipped app needs all three.

### Loading skeletons over spinners

```tsx
// BAD: generic spinner (content jumps when it loads)
if (isLoading) return <Spinner />;

// GOOD: skeleton that matches the shape of the content
if (isLoading) return (
  <div className="space-y-3">
    <div className="h-12 bg-gray-100 rounded animate-pulse" />
    <div className="h-12 bg-gray-100 rounded animate-pulse" />
    <div className="h-12 bg-gray-100 rounded animate-pulse" />
  </div>
);
```

### Empty states are features

```tsx
// BAD
if (machines.length === 0) return <p>No data.</p>;

// GOOD
if (machines.length === 0) return (
  <div className="text-center py-12">
    <h3 className="text-lg font-medium">No machines added yet</h3>
    <p className="text-sm text-gray-500 mt-1">
      Add your first machine to start tracking your fleet.
    </p>
    <Button onClick={() => router.push("/machines/new")} className="mt-4">
      Add Machine
    </Button>
  </div>
);
```

## Form Handling

### Controlled forms with validation

```tsx
function BookingForm({ onSubmit }: { onSubmit: (data: BookingInput) => void }) {
  const [form, setForm] = useState({ machineId: "", customerId: "", startDate: "", endDate: "" });
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  function validate() {
    const errs: Record<string, string> = {};
    if (!form.machineId) errs.machineId = "Select a machine";
    if (!form.customerId) errs.customerId = "Select a customer";
    if (!form.startDate) errs.startDate = "Start date required";
    if (!form.endDate) errs.endDate = "End date required";
    if (form.startDate && form.endDate && form.startDate >= form.endDate) {
      errs.endDate = "End date must be after start date";
    }
    return errs;
  }

  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    const errs = validate();
    setErrors(errs);
    if (Object.keys(errs).length > 0) return;

    setIsSubmitting(true);
    try {
      await onSubmit(form);
    } catch (err) {
      setErrors({ form: err instanceof Error ? err.message : "Something went wrong" });
    } finally {
      setIsSubmitting(false);
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      {errors.form && <div role="alert" className="error-banner">{errors.form}</div>}
      {/* fields with individual error display */}
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Creating..." : "Create Booking"}
      </button>
    </form>
  );
}
```

Key points: validation on submit (not just on blur), disabled button during submit, form-level error for server errors, field-level errors for validation.

## Performance: Don't Optimize Prematurely

### When useMemo/useCallback matter

```tsx
// UNNECESSARY: simple computation
const total = useMemo(() => items.length, [items]); // Just use items.length

// NECESSARY: expensive computation
const sortedBookings = useMemo(
  () => bookings.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt)),
  [bookings]
);

// UNNECESSARY: callback passed to HTML element
const handleClick = useCallback(() => setOpen(true), []); // onClick on <button> doesn't need this

// NECESSARY: callback passed to memoized child component
const handleSelect = useCallback((id: string) => {
  setSelected(id);
}, []);
// Only matters if MachineCard is wrapped in React.memo
```

**Rule: don't memoize unless you've measured a performance problem.**
