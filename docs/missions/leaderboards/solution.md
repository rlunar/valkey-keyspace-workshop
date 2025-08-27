[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 4️⃣: Death Star Leaderboards with Sorted Sets__

# 🚀 Bonus Challenges Solutions

## **Challenge 1: Rank Neighborhood Analysis**  

*"Show me the pilots around my rank, young Skywalker"*

Objective: Get the pilots immediately above and below a specific pilot's rank

Hint Commands: [`ZREVRANGE`](https://valkey.io/commands/zrevrange/) with calculated positions based on [`ZREVRANK`](https://valkey.io/commands/zrevrank/)

## **Challenge 2: Score Increment Operations**

*"Your skills have doubled since last we met, Count Dooku"*

Objective: Increase pilot scores relatively and handle score bonuses

Hint Commands: [`ZINCRBY`](https://valkey.io/commands/zincrby/)

## **Challenge 3: Leaderboard Segmentation**  

*"Separate the Padawans from the Jedi Masters"*

Objective: Create multiple skill-based leaderboards and cross-reference rankings

Hint Commands: [`ZINTERSTORE`](https://valkey.io/commands/zinterstore/), [`ZUNIONSTORE`](https://valkey.io/commands/zunionstore/)

## **Challenge 4: Historical Rankings**

*"A long time ago, in a galaxy far, far away..."*

Objective: Track pilot performance over time and compare historical vs current rankings

Hint Commands: [`ZREMRANGEBYRANK`](https://valkey.io/commands/zremrangebyrank/), [`ZREMRANGEBYSCORE`](https://valkey.io/commands/zremrangebyscore/)

## **Challenge 5: Elite Squadron Selection**

*"Only the best pilots for this Death Star assault"*

Objective: Remove low-performing pilots and maintain a maximum leaderboard size

Hint Commands: [`ZPOPMIN`](https://valkey.io/commands/zpopmin/), [`ZPOPMAX`](https://valkey.io/commands/zpopmax/)

## ➡️ Next: [Mission 5️⃣: Hyperdrive Rate Limiters with Lua Scripts](../rate-limiters/README.md)

[Mission 4️⃣ Challenge Solutions](../leaderboards/solution.md)
