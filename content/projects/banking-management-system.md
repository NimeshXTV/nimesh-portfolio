---
title: "Banking Management System"
description: "1,000 concurrent transfers, zero money created or destroyed — JDBC transactions proven under fire."
stack: ["Java", "JDBC", "MySQL"]
githubUrl: "https://github.com/NimeshXTV/Banking-Management-System"
order: 2
publishDate: 2026-08-15
---

## What I built

A console banking system built in Java over raw JDBC. Accounts, deposits,
PIN-gated withdrawals, and transfers wrapped in real transactions:
`setAutoCommit(false)` → credit recipient, debit sender → commit only if both
updates land, otherwise rollback.

## The benchmark

I created a harness that slams a real MySQL 8 database with worst-case
contention: 8 worker threads doing 125 transfers of ₹5 each, all hitting the
same two rows simultaneously.

## Results

- **1,000/1,000 transfers committed, zero rolled back**
- **Final balances: exactly ₹5,000.00 and ₹15,000.00** — not approximately, to
  the paisa
- **Rollback path verified separately** — a failed transfer backs out cleanly
- **Throughput: 451 transfers/sec** through real row locks

## The honest limitation

The balance check runs before the transaction. Two concurrent transfers can
both pass and jointly overdraw. The fix is one line: move the guard into the
transaction with a conditional `UPDATE`. The commit/rollback machinery already
handles it correctly.

<a href="https://github.com/NimeshXTV/Banking-Management-System" target="_blank" class="cactus-link font-bold">View the source on GitHub →</a>
