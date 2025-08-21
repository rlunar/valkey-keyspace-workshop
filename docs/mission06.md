## Mission 6: Real-time Battle Analytics (12 minutes)  
*"I've got a bad feeling about this... let me check the analytics"*

### 🌟 What are Real-time Analytics?
Real-time analytics are like having C-3PO constantly calculating odds during battle - processing data as it happens to provide immediate insights. Whether tracking Death Star vulnerability during the Battle of Endor or monitoring Rebel fleet effectiveness, real-time analytics turn data streams into actionable intelligence.

**Why Real-time Analytics?**
- **Immediate insights**: Make decisions based on current data
- **Trend detection**: Spot patterns as they emerge  
- **Operational monitoring**: Track system health and performance
- **User behavior**: Understand interactions as they happen
- **Alerting**: Trigger responses to critical events

**Analytics Components:**
- **Counters**: Simple numeric tracking (shots fired, users online)
- **Unique counting**: Distinct items without storing full sets  
- **Rankings**: Top performers and trending items
- **Time series**: Metrics over time periods
- **Aggregations**: Sums, averages, percentiles across datasets

### The Challenge
Admiral Ackbar needs real-time battle analytics to detect traps. We need to count unique rebel ships, track total shots fired, and maintain top weapon effectiveness.

### Valkey Solution: Counters + HyperLogLogs + Sorted Sets
```bash
# Simple counters for battle metrics
INCR battle:shots_fired
INCR battle:hits_landed
INCR battle:shots_by_weapon:laser  
INCR battle:shots_by_weapon:torpedo
INCR battle:shots_by_weapon:ion_cannon

# Track unique ships with HyperLogLog (memory efficient)
PFADD battle:unique_rebel_ships "rebel_xwing_1" "rebel_ywing_2" "rebel_bwing_1"
PFADD battle:unique_imperial_ships "tie_fighter_1" "tie_interceptor_1" "star_destroyer_1"

# Count unique participants without storing full member list
PFCOUNT battle:unique_rebel_ships
PFCOUNT battle:unique_imperial_ships

# Weapon effectiveness leaderboard (damage per shot)
ZINCRBY battle:weapon_effectiveness 150 "laser_cannon"
ZINCRBY battle:weapon_effectiveness 500 "proton_torpedo"  
ZINCRBY battle:weapon_effectiveness 75 "ion_cannon"
ZINCRBY battle:weapon_effectiveness 300 "turbolaser"

# Real-time battle dashboard data
GET battle:shots_fired
GET battle:hits_landed
PFCOUNT battle:unique_rebel_ships
ZREVRANGE battle:weapon_effectiveness 0 4 WITHSCORES

# Calculate hit ratio (requires application logic combining values)
GET battle:hits_landed
GET battle:shots_fired

# Track battle events over time (time-based keys)
INCR battle:2024_12_10_15:shots_fired
INCR battle:2024_12_10_15:hits_landed
```

### 🚀 Bonus Challenges

**Challenge 1: Battle Efficiency Metrics**
*"These blast points... too accurate for Sand People"*

Calculate hit ratios and weapon accuracy using multiple counters
- Commands: [`MGET`](https://valkey.io/commands/mget/) to retrieve multiple metrics, combine with [`EVAL`](https://valkey.io/commands/eval/) for calculations

**Challenge 2: Fleet Composition Analysis**  
*"Analyze the enemy fleet composition, Admiral"*

Combine unique ship counts from multiple battles and calculate totals
- Commands: [`PFMERGE`](https://valkey.io/commands/pfmerge/) to combine HyperLogLogs, [`PFADD`](https://valkey.io/commands/pfadd/) bulk operations

**Challenge 3: Top Performers Dashboard**
*"Who are our ace pilots and worst weapons?"*

Get top and bottom weapon effectiveness, plus specific weapon rankings
- Commands: [`ZREVRANGE`](https://valkey.io/commands/zrevrange/) for top performers, [`ZRANGE`](https://valkey.io/commands/zrange/) for bottom, [`ZRANK`](https://valkey.io/commands/zrank/)/[`ZSCORE`](https://valkey.io/commands/zscore/) for specific weapons

**Challenge 4: Time-Series Battle Analysis**
*"Show me how this battle unfolded over time"*

Track metrics across multiple time windows and calculate trends
- Commands: Use time-based keys with [`KEYS`](https://valkey.io/commands/keys/) patterns, [`SORT`](https://valkey.io/commands/sort/) for chronological analysis  

**Challenge 5: Multi-Battle Campaign Analytics**
*"Compare this battle to the Battle of Hoth"*

Aggregate analytics across multiple battles and campaigns
- Commands: [`ZUNIONSTORE`](https://valkey.io/commands/zunionstore/) to combine weapon effectiveness across battles, pattern-based key operations

**Challenge 6: Alert System Integration**  
*"It's a trap! Trigger alerts when metrics exceed thresholds"*

Set up threshold monitoring and automated alerts
- Commands: [`EVAL`](https://valkey.io/commands/eval/) scripts that check thresholds and trigger actions, [`PUBLISH`](https://valkey.io/commands/publish/) for notifications

---
