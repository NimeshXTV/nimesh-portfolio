---
title: "Banking Management System"
description: "Java • Spring Boot • PostgreSQL • 1,000 concurrent deposits validated with row-level locking"
stack: ["Java", "JDBC", "MySQL"]
githubUrl: "https://github.com/NimeshXTV/Banking-Management-System_"
order: 2
publishDate: 2026-08-15
---

## What I built

A REST banking API built with Java 17, Spring Boot 3.2, JPA, and PostgreSQL. It supports account creation, deposits, withdrawals, transfers, and transaction history, with validation and centralized exception handling.

## The concurrency test

I wanted to make sure simultaneous deposits couldn't overwrite each other. I wrote a test that sends 1,000 concurrent $10 deposits to the same account using 20 worker threads, backed by a real PostgreSQL database running in Docker.

## Results
- 1,000/1,000 deposits completed successfully
- Final balance: exactly $10,001.00
- 1,001 transaction records — including the initial deposit
- Balance and transaction history remained consistent under concurrent writes
- Row-level locking handled the updates with PESSIMISTIC_WRITE

## How I handled it

The account is locked before its balance is read and updated using a JPA PESSIMISTIC_WRITE query. For transfers, both account rows are locked in ID order so two opposing transfers don't wait on each other indefinitely.

## What I learned

The interesting part wasn't building the endpoints — it was making the balance update safe when multiple operations hit the same database row at the same time. The concurrency test gave me a way to verify that behavior against PostgreSQL rather than just assuming the locking worked.

<a href="https://github.com/NimeshXTV/Banking-Management-System_" target="_blank" class="cactus-link font-bold">View the source on GitHub →</a>
