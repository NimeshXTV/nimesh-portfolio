---
title: "Multithreaded Web Server"
description: "A TCP echo server benchmarked to 39,000 msg/sec — proving that five threads outperforms fifty, and why thread-per-connection fails under load."
stack: ["Java", "Sockets", "ExecutorService"]
githubUrl: "https://github.com/NimeshXTV/multithreaded-web-server"
order: 1
publishDate: 2026-08-15
---

## What I built

A TCP echo server in pure Java using `ServerSocket` and `ExecutorService`. No
frameworks. Each client gets handed from the accept loop to a fixed thread
pool — one line in, one line out, connection closed.

## The benchmark

I created a custom load tester ramping concurrency from 50 to 800 simultaneous
clients, comparing three server variants with identical handlers: the repo as
shipped (pool of 5), a pool of 50, and thread-per-connection.

## Results at 800 clients

- **Pool-5: 32,237 msg/sec, zero failures**
- **Pool-50: 30,038 msg/sec, zero failures**
- **Thread-per-connection: 14,151 msg/sec, 286 failed connections**

## What the numbers say

Five threads saturate the workload. More threads add overhead without gain.
Thread-per-connection loses ~55% throughput because the machine can't sustain
~1,400 thread creates/destroys per second under connection churn.

Peak throughput across all levels: **39,382 msg/sec** (pool-5 at 200
concurrency, zero errors at every level).

<a href="https://github.com/NimeshXTV/multithreaded-web-server" target="_blank" class="cactus-link font-bold">View the source on GitHub →</a>
