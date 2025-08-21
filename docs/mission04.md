## Mission 4: Death Star Leaderboards with Sorted Sets (12 minutes)
*"That's no moon... that's a sorted set!"*

### 🌟 What are Leaderboards?
Leaderboards are like the Imperial Academy's pilot rankings - automatically sorted lists where performance determines position. Whether tracking TIE fighter kill counts or Podracing lap times, sorted sets maintain real-time rankings without manual sorting.

**Why Use Leaderboards?**
- **Gamification**: Drive engagement through competition
- **Performance tracking**: Monitor metrics in real-time
- **Automatic sorting**: No manual rank calculations needed
- **Range queries**: Find top performers or specific score ranges
- **Efficient updates**: O(log N) insertions and updates

**Leaderboard Applications:**
- **Gaming**: Player scores and achievements  
- **E-commerce**: Product ratings and reviews
- **Analytics**: Performance metrics and KPIs
- **Social**: User reputation and activity scores

### The Challenge
The Empire needs to rank TIE fighter pilots by their hit accuracy. Even storm troopers deserve recognition (when they actually hit something).

### Valkey Solution: Sorted Sets
```bash
# Add pilots with accuracy scores (score is the ranking value)
ZADD pilot_leaderboard 98.5 "Darth_Vader"
ZADD pilot_leaderboard 99.9 "Luke_Skywalker"  
ZADD pilot_leaderboard 87.3 "Wedge_Antilles"
ZADD pilot_leaderboard 12.3 "Stormtrooper_TK421"
ZADD pilot_leaderboard 15.7 "Stormtrooper_TK422"
ZADD pilot_leaderboard 94.2 "Biggs_Darklighter"

# Get top pilots (highest scores first)
ZREVRANGE pilot_leaderboard 0 4 WITHSCORES

# Get bottom performers (lowest scores)
ZRANGE pilot_leaderboard 0 2 WITHSCORES

# Find specific pilot's rank (0-based, highest score = rank 0)
ZREVRANK pilot_leaderboard "Luke_Skywalker"
ZRANK pilot_leaderboard "Stormtrooper_TK421"

# Get pilot's score
ZSCORE pilot_leaderboard "Darth_Vader"

# Update scores (pilot improvement after training)
ZADD pilot_leaderboard 45.6 "Stormtrooper_TK421"

# Get pilots within accuracy range (elite squadron criteria)
ZRANGEBYSCORE pilot_leaderboard 90 100 WITHSCORES

# Count pilots in score range
ZCOUNT pilot_leaderboard 90 100
```

### 🚀 Bonus Challenges

**Challenge 1: Rank Neighborhood Analysis**  
*"Show me the pilots around my rank, young Skywalker"*

Get the pilots immediately above and below a specific pilot's rank
- Commands: [`ZREVRANGE`](https://valkey.io/commands/zrevrange/) with calculated positions based on [`ZREVRANK`](https://valkey.io/commands/zrevrank/)

**Challenge 2: Score Increment Operations**
*"Your skills have doubled since last we met, Count Dooku"*

Increase pilot scores relatively and handle score bonuses
- Commands: [`ZINCRBY`](https://valkey.io/commands/zincrby/)

**Challenge 3: Leaderboard Segmentation**  
*"Separate the Padawans from the Jedi Masters"*

Create multiple skill-based leaderboards and cross-reference rankings
- Commands: [`ZINTERSTORE`](https://valkey.io/commands/zinterstore/), [`ZUNIONSTORE`](https://valkey.io/commands/zunionstore/)

**Challenge 4: Historical Rankings**
*"A long time ago, in a galaxy far, far away..."*

Track pilot performance over time and compare historical vs current rankings
- Commands: [`ZREMRANGEBYRANK`](https://valkey.io/commands/zremrangebyrank/), [`ZREMRANGEBYSCORE`](https://valkey.io/commands/zremrangebyscore/)

**Challenge 5: Elite Squadron Selection**
*"Only the best pilots for this Death Star assault"*

Remove low-performing pilots and maintain a maximum leaderboard size
- Commands: [`ZPOPMIN`](https://valkey.io/commands/zpopmin/), [`ZPOPMAX`](https://valkey.io/commands/zpopmax/)

---
