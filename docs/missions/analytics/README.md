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

1. Simple counters for battle metrics

```bash
INCR battle:shots_fired
```

```bash
INCR battle:hits_landed
```

```bash
INCR battle:shots_by_weapon:laser  
```

```bash
INCR battle:shots_by_weapon:torpedo
```

```bash
INCR battle:shots_by_weapon:ion_cannon
```

2. Track unique ships with HyperLogLog (memory efficient)

```bash
PFADD battle:unique_rebel_ships "rebel_xwing_1" "rebel_ywing_2" "rebel_bwing_1"
```

```bash
PFADD battle:unique_imperial_ships "tie_fighter_1" "tie_interceptor_1" "star_destroyer_1"
```

3. Count unique participants without storing full member list

```bash
PFCOUNT battle:unique_rebel_ships
```

```bash
PFCOUNT battle:unique_imperial_ships
```

4. Weapon effectiveness leaderboard (damage per shot)

```bash
ZINCRBY battle:weapon_effectiveness 150 "laser_cannon"
```

```bash
ZINCRBY battle:weapon_effectiveness 500 "proton_torpedo"  
```

```bash
ZINCRBY battle:weapon_effectiveness 75 "ion_cannon"
```

```bash
ZINCRBY battle:weapon_effectiveness 300 "turbolaser"
```

5. Real-time battle dashboard data

```bash
GET battle:shots_fired
```

```bash
GET battle:hits_landed
```

```bash
PFCOUNT battle:unique_rebel_ships
```

```bash
ZREVRANGE battle:weapon_effectiveness 0 4 WITHSCORES
```

6. Calculate hit ratio (requires application logic combining values)
```bash
GET battle:hits_landed
```

```bash
GET battle:shots_fired
```

7. Track battle events over time (time-based keys)

```bash
INCR battle:2024_12_10_15:shots_fired
```

```bash
INCR battle:2024_12_10_15:hits_landed
```
