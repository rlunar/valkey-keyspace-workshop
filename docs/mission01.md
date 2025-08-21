## Mission 1: Hyperdrive Caching with Strings (16 minutes)
*"Punch it, Chewie!" - Making your app faster than light*

### 🌟 What is Caching?
Caching is like having R2-D2's memory banks storing frequently accessed information close at hand for lightning-fast retrieval. Instead of calculating the hyperdrive coordinates every time, you store them for instant access when escaping Imperial forces.

**Why Cache?**
- **Speed**: Sub-millisecond data access vs. disk based database queries (100-1000x faster ⚡)
- **Efficiency**: Reduce database load like reducing strain on the hyperdrive
- **Scalability**: Handle more requests than a busy cantina (millions of ops/sec 📈)
- **Cost savings**: Reduce expensive database operations and infrastructure costs (save those credits 💸)
- **User experience**: Faster page loads = happier users (like satisfied cantina customers 😃)


### 🛸 Common Caching Patterns Deep Dive


#### **1. Cache-Aside (Lazy Loading)**
*"R2-D2 only loads Death Star plans when Luke actually needs them"*

**How it works:**
- Application checks cache first
- On cache miss ❌, fetch from database 💽
- Store result in cache ⚡ for next time
- Application manages all cache operations 🧠

**Best for:** Read-heavy workloads ♻️, unpredictable access patterns

```bash
# Cache-aside pattern example
# 1. Check cache first
GET death_star_plans:weakness_analysis

# 2. Cache miss - fetch from "database" and cache result
SET death_star_plans:weakness_analysis "Thermal exhaust port vulnerability confirmed" EX 3600

# 3. Subsequent reads hit cache
GET death_star_plans:weakness_analysis
```

Wow, wow, take it easy! What just happened? Alright, let's go step by step and understand each of the previous commands.

**Pros:** ✅
- Only cache what's actually needed
- Cache failures don't break the application
- Data consistency is manageable

**Cons:** ❗ 
- Initial requests are slow (cache miss penalty)
- Application code complexity increases (Developers test as Jedi Masters)
- Potential for stale data (Like ancient languages that haven't been spoken for thousands of years)


#### **2. Write-Through**
*"C-3PO updates both his memory banks and the central Rebel database simultaneously"*

**How it works:**
- Write to cache and database at the same time 🔄
- Every write operation updates both stores 🔃
- Reads always hit cache (if cache is available) 
- Strong consistency between cache and database 💪

**Best for:** Applications requiring data consistency, moderate write loads

```bash
# Write-through simulation (application would handle database write)
# Update both cache and database in same operation
SET rebel_base_location:hoth "Echo Base, coordinates 39.5991° N, 2.9238° E" EX 7200

# Reads are always fast and consistent
GET rebel_base_location:hoth
```

**Pros:** ✅
- Data consistency guaranteed
- Fast reads after writes
- Simple read logic

**Cons:** ❗
- Write latency increases (dual writes)
- Cache stores data that might never be read
- More complex failure handling


#### **3. Write-Behind (Write-Back)**
*"Han Solo updates his pilot log in the Falcon's computer first, syncs with Rebel command later"*

**How it works:**
- Write to cache immediately
- Database write happens asynchronously later
- Fastest write performance
- Risk of data loss if cache fails before sync

**Best for:** Write-heavy applications, acceptable eventual consistency

```bash
# Write-behind pattern - immediate cache update
SET pilot_log:han_solo "Completed Kessel Run in 12 parsecs" EX 1800

# Application separately handles async database write
# Cache provides immediate read access
GET pilot_log:han_solo
```

**Pros:** ✅
- Fastest write performance
- Reduced database load
- Can batch database writes for efficiency

**Cons:** ❗
- Risk of data loss
- Complex consistency management
- Difficult error handling


#### **4. Refresh-Ahead**
*"R2-D2 preemptively updates astrogation charts before they expire"*

**How it works:**
- Automatically refresh cache before expiration
- Background process monitors cache TTL
- Prevents cache misses on popular data
- Requires prediction of data access patterns

**Best for:** Predictable access patterns, critical performance requirements

```bash
# Refresh-ahead pattern simulation
SET hyperdrive_routes:tatooine_to_alderaan "Safe hyperspace lanes verified" EX 3600

# Background process would refresh before expiration
# Check TTL and refresh when < threshold
TTL hyperdrive_routes:tatooine_to_alderaan

# Refresh if TTL < 300 seconds (5 minutes)
SET hyperdrive_routes:tatooine_to_alderaan "Routes updated and verified" EX 3600
```

**Pros:** ✅
- Eliminates cache misses for popular data
- Consistent fast performance
- Predictable load patterns

**Cons:** ❗
- Complex implementation
- May refresh unused data
- Requires access pattern prediction


### 🎯 Cache Strategy Selection Guide

**Choose Cache-Aside when:**
- Read-heavy workloads (social media feeds, product catalogs)
- Unpredictable access patterns
- Can tolerate some read latency on misses

**Choose Write-Through when:**
- Strong consistency required (financial transactions, user profiles)
- Read performance is critical
- Moderate write volume

**Choose Write-Behind when:**
- Write performance is critical (logging, analytics, gaming leaderboards)
- Can tolerate eventual consistency
- High write volume

**Choose Refresh-Ahead when:**
- Predictable access patterns (popular content, dashboards)
- Zero tolerance for cache misses
- Can invest in complex cache management


### The Challenge
The Rebel Alliance needs to cache Imperial fleet positions across multiple star systems. Every millisecond counts when outrunning TIE fighters!

### Solution: Use Valkey Strings Luke
```bash
# Connect to Valkey (ensure server is running)
valkey-cli

# Cache fleet positions with expiration (TTL in seconds)
SET imperial_fleet:tatooine "3 Star Destroyers, sector 7G" EX 300

# Retrieve cached data faster than R2-D2 accessing Death Star plans
GET imperial_fleet:tatooine

# Check remaining TTL
TTL imperial_fleet:tatooine

# Cache multiple systems
SET imperial_fleet:hoth "1 Super Star Destroyer, 6 Star Destroyers" EX 600
SET imperial_fleet:endor "Shield generator station, 2 Star Destroyers" PX 300

# Retrieve all fleet data
GET imperial_fleet:tatooine
GET imperial_fleet:hoth  
GET imperial_fleet:endor

# What happened with Endor's data? Why did we get (nil)?
TTL imperial_fleet:tatooine
```

If you get a response (integer) -2 means the TTL has expired, note the PX parameter in the SET command (this means 300 milliseconds), by the time we try to read the data has already been expired from memory.

Beware of the duration of items in the Cache, sometimes you do want to evict them in less than a second, sometimes less than 1 minute, 1 hour or maybe exactly at midnight, for this you can specify TTL 


### 🚀 Bonus Challenges

**Challenge 1: Cache Invalidation Pattern**
*"Your lack of cache invalidation disturbs me" - Vader*

Hint: Use pattern matching to clear all Imperial fleet caches at once
- Commands: [`SCAN`](https://valkey.io/commands/scan/), [`UNLINK`](https://valkey.io/commands/unlink/)

**Challenge 2: Atomic Updates**
*"I find your race condition disturbing"*

Update fleet strength only if the current value matches expected
- Commands: [`GET`](https://valkey.io/commands/get/), [`SET`](https://valkey.io/commands/set/) with conditions

**Challenge 3: Bulk Operations**
*"These are not the slow operations you're looking for"*

Set multiple fleet positions in a single atomic operation
- Commands: [`MSET`](https://valkey.io/commands/mset/), [`MGET`](https://valkey.io/commands/mget/)

**Challenge 4: Conditional Caching**
*"Only store this intelligence if no one else has reported it"*

Cache fleet data only if the key doesn't exist yet
- Commands: [`SET`](https://valkey.io/commands/set/) with `NX` option, [`SETNX`](https://valkey.io/commands/setnx/)

---

**Congratulations** young Padawan! You've helped win this battle, move onto the next mission; the clone wars wil continue for longer.

[Challennge Solutions](docs/mission01-solutions.md)
