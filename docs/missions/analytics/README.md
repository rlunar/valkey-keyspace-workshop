[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

# Mission 6️⃣: Real-time Battle Analytics

*"I've got a bad feeling about this... let me check the analytics"*

## 🌟 What are Real-time Analytics?

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

### Solution: Valkey Counters + Valkey HyperLogLogs + Valkey Sorted Sets

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
