---
title: "Ticket Booking System"
description: "100 threads raced for the last seat 1,000 times. The unsynchronized code double-booked in 24 rounds and crashed in 5. One word fixed it."
stack: ["Java", "Collections", "File I/O"]
githubUrl: "https://github.com/NimeshXTV/ticket-booking-system"
order: 4
publishDate: 2026-08-15
---

## What I built

A CLI train-booking app in Java where users log in, select routes, and book
seats. Bookings persist to disk so seats can't be resold after restart. Built
with a race harness that deliberately triggers the concurrency bug.

## The bug

Seat allocation is check-then-act: Check "is seat 5 free?" → YES → Remove
seat 5. In single-threaded code this is safe by accident. Under concurrency,
the gap becomes a race condition: both threads pass the check before either
removes, so both book the same seat.

## Race test results

- Unsynchronized: 24 double-bookings, 1,028 bookings (28 phantom seats), 5 JDK
  crashes
- Synchronized fix: 0 double-bookings, exactly 1,000 bookings, 0 crashes

## The fix

The fix is one word of Java: `synchronized`. Makes check-and-remove atomic.

```java
public synchronized Booking bookSeat(Train train, User user, int seat) {
    if (!train.getSeats().contains(seat)) return null;
    train.getSeats().remove(Integer.valueOf(seat));
    return new Booking(user, train, seat);
}
```

Proven by the same harness: 1,000 rounds, zero anomalies. The race and fix
were demonstrated in a benchmark copy — the public repo still ships the
unsynchronized version, one commit away from the fix.

<a href="https://github.com/NimeshXTV/ticket-booking-system" target="_blank" class="cactus-link font-bold">View the source on GitHub →</a>
