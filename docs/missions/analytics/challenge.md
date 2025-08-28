[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 6️⃣: Real-time Battle Analytics__

# 🚀 Bonus Challenges

## **Challenge 1: Battle Efficiency Metrics**

*"These blast points... too accurate for Sand People"*

Objective: Calculate hit ratios and weapon accuracy using multiple counters

Hint Commands: [`MGET`](https://valkey.io/commands/mget/) to retrieve multiple metrics, combine with [`EVAL`](https://valkey.io/commands/eval/) for calculations

## **Challenge 2: Fleet Composition Analysis**  

*"Analyze the enemy fleet composition, Admiral"*

Objective: Combine unique ship counts from multiple battles and calculate totals

Hint Commands: [`PFMERGE`](https://valkey.io/commands/pfmerge/) to combine HyperLogLogs, [`PFADD`](https://valkey.io/commands/pfadd/) bulk operations

## **Challenge 3: Top Performers Dashboard**

*"Who are our ace pilots and worst weapons?"*

Objective: Get top and bottom weapon effectiveness, plus specific weapon rankings

Hint Commands: [`ZREVRANGE`](https://valkey.io/commands/zrevrange/) for top performers, [`ZRANGE`](https://valkey.io/commands/zrange/) for bottom, [`ZRANK`](https://valkey.io/commands/zrank/)/[`ZSCORE`](https://valkey.io/commands/zscore/) for specific weapons

## **Challenge 4: Time-Series Battle Analysis**

*"Show me how this battle unfolded over time"*

Objective: Track metrics across multiple time windows and calculate trends

Hint Commands: Use time-based keys with [`KEYS`](https://valkey.io/commands/keys/) patterns, [`SORT`](https://valkey.io/commands/sort/) for chronological analysis  

## **Challenge 5: Multi-Battle Campaign Analytics**

*"Compare this battle to the Battle of Hoth"*

Objective: Aggregate analytics across multiple battles and campaigns

Hint Commands: [`ZUNIONSTORE`](https://valkey.io/commands/zunionstore/) to combine weapon effectiveness across battles, pattern-based key operations

## **Challenge 6: Alert System Integration**  

*"It's a trap! Trigger alerts when metrics exceed thresholds"*

Objective: Set up threshold monitoring and automated alerts

Hint Commands: [`EVAL`](https://valkey.io/commands/eval/) scripts that check thresholds and trigger actions, [`PUBLISH`](https://valkey.io/commands/publish/) for notifications


---

[Missions](../../missions.md) | [Final Mission Debrief](../../debrief.md)
