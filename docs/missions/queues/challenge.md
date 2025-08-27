[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 3️⃣: Galactic Task Queues with Lists__

# 🚀 Bonus Challenges [Optional]

## **Challenge 1: Mission Priority Reordering**

*"I am Groot" - Task prioritization*

![I am Groot](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcGp0dXRncHZlM3Uwd3J6NTRjeXZuem1hdW11OTBuN2ttdzB3eDA2dSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/l4FGrYKtP0pBGpBAY/giphy.gif)

Objective: Move a standard mission to urgent queue and insert at specific position from existing queues: `missions:standard` and `missions:urgent`.

Hint Commands: [`LREM`](https://valkey.io/commands/lrem/), [`LINSERT`](https://valkey.io/commands/linsert/), [`LSET`](https://valkey.io/commands/lset/)

## **Challenge 2: Mission Queue Monitoring**

*"Rocket's mission queue dashboard"*

![Rocket-Racoon](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExYWZpcW9qcXBhejYwZzlwYWhnaGZrdWN2bWpzeXRpMzUwbXJobWtycCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oKIPzVXlzxhAWamNW/giphy.gif)

Objective: Get queue statistics and find specific missions without removing them from `missions:standard` queue.

Hint Commands: [`LPOS`](https://valkey.io/commands/lpos/), [`LLEN`](https://valkey.io/commands/llen/) for queue size limits.

## ➡️ Next: [Mission 4️⃣: Death Star Leaderboards with Sorted Sets](../leaderboards/README.md)

[Mission 3️⃣ Challenge Solutions](../queues/solution.md)
