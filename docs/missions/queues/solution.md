[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 3️⃣: Galactic Task Queues with Lists__

# 🚀 Bonus Challenges Solutions

## **Challenge 1: Mission Priority Reordering**

*"I am Groot" - Task prioritization*

![I am Groot](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcGp0dXRncHZlM3Uwd3J6NTRjeXZuem1hdW11OTBuN2ttdzB3eDA2dSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/l4FGrYKtP0pBGpBAY/giphy.gif)

Objective: Move an item from `missions:standard` (rightmost) and insert it at the left side of `missions:urgent`.

Hint Commands: [`RPOPLPUSH`](https://valkey.io/commands/rpoplpush/)

__Solution__

```bash
RPOPLPUSH missions:standard missions:urgent
```

Response:
> "Fix Milano's navigation system"

List all the elements in the `missions:urgent` queue:

```bash
LRANGE missions:urgent 0 -1
```

Response:
>
> 1) "Fix Milano's navigation system"
> 2) "Stop Ronan from destroying Xandar"
>

## **Challenge 2: Mission Queue Monitoring**

*"Rocket's mission queue dashboard"*

![Rocket-Racoon](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExYWZpcW9qcXBhejYwZzlwYWhnaGZrdWN2bWpzeXRpMzUwbXJobWtycCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oKIPzVXlzxhAWamNW/giphy.gif)

Objective: Get queue statistics and find specific missions without removing them from `missions:standard` queue.

Hint Commands: [`LPOS`](https://valkey.io/commands/lpos/), [`LLEN`](https://valkey.io/commands/llen/) for queue size limits.

## ➡️ Next: [Mission 4️⃣: Death Star Leaderboards with Sorted Sets](../leaderboards/README.md)

[Mission 3️⃣ Challenge Solutions](../queues/solution.md)
