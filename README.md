# LEETCODE-Arrays-731
Let's walk through a dry run of the `MyCalendarTwo` class to see how it works. This class allows you to book events, ensuring no time slot is triple-booked (i.e., two events can overlap, but a third event cannot overlap with them).

The main idea is to use a `TreeMap` (`tm`) to track the start and end times of the events. Here's the step-by-step process of how it works:

### Initialization:
- We initialize an empty `TreeMap<Integer, Integer> tm = new TreeMap<>()`.
- The `TreeMap` will store the count of starts (+1) and ends (-1) of the events at specific times.

### Method: `book(int start, int end)`
This method tries to book an event from `start` to `end` and returns `true` if the booking can be made without causing a triple overlap. If a triple overlap would occur, it cancels the booking and returns `false`.

### Example Run:

#### First booking: `book(10, 20)`
1. `tm.put(10, tm.getOrDefault(10, 0) + 1);` → `tm.put(10, 1)` (This means an event starts at 10).
2. `tm.put(20, tm.getOrDefault(20, 0) - 1);` → `tm.put(20, -1)` (This means an event ends at 20).
3. Now, `tm = {10=1, 20=-1}`.
4. Now we check the event count `s`. We iterate over the `values()` of the `TreeMap`:
   - At time `10`, `s = 1`. (No triple overlap yet.)
   - At time `20`, `s = 0`. (Still no triple overlap.)
5. No overlap greater than 2 is found, so we return `true`.

#### Second booking: `book(15, 25)`
1. `tm.put(15, tm.getOrDefault(15, 0) + 1);` → `tm.put(15, 1)` (Event starts at 15).
2. `tm.put(25, tm.getOrDefault(25, 0) - 1);` → `tm.put(25, -1)` (Event ends at 25).
3. Now, `tm = {10=1, 15=1, 20=-1, 25=-1}`.
4. We check the event count `s`:
   - At time `10`, `s = 1`.
   - At time `15`, `s = 2`.
   - At time `20`, `s = 1`.
   - At time `25`, `s = 0`.
5. No overlap greater than 2 is found, so we return `true`.

#### Third booking: `book(17, 22)`
1. `tm.put(17, tm.getOrDefault(17, 0) + 1);` → `tm.put(17, 1)` (Event starts at 17).
2. `tm.put(22, tm.getOrDefault(22, 0) - 1);` → `tm.put(22, -1)` (Event ends at 22).
3. Now, `tm = {10=1, 15=1, 17=1, 20=-1, 22=-1, 25=-1}`.
4. We check the event count `s`:
   - At time `10`, `s = 1`.
   - At time `15`, `s = 2`.
   - At time `17`, `s = 3` (Now we have a triple overlap!).
5. A triple overlap is found at `s = 3`, so we revert the changes:
   - `tm.put(17, tm.get(17) - 1);` → `tm.put(17, 0)`.
   - `tm.put(22, tm.get(22) + 1);` → `tm.put(22, 0)`.
6. Now, `tm = {10=1, 15=1, 20=-1, 25=-1}`.
7. Since a triple overlap was detected, we return `false`.

### Summary:
- The first two bookings succeed (`true`), and the third booking fails due to a triple overlap (`false`).

The class uses a greedy strategy and the TreeMap to efficiently track intervals and detect overlaps by adjusting the event counts at start and end times.
