## Mission 5: Hyperdrive Rate Limiters with Lua Scripts (12 minutes)
*"The hyperdrive is damaged, Captain. We need to limit our jumps!"*

### 🌟 What is Rate Limiting?
Rate limiting is like controlling hyperdrive usage to prevent engine overload. Just as the Millennium Falcon can't make unlimited jumps without risking hull damage, APIs and services need protection from too many requests. It's the difference between controlled hyperspace navigation and catastrophic system failure.

**Why Rate Limit?**
- **Resource protection**: Prevent system overload
- **Fair usage**: Ensure equitable access for all users  
- **Security**: Mitigate abuse and attacks
- **Cost control**: Manage resource consumption
- **Quality of service**: Maintain performance for everyone

**Rate Limiting Strategies:**
- **Fixed window**: X requests per minute/hour
- **Sliding window**: Rolling time window for smoother limiting
- **Token bucket**: Burst capability with sustained rate
- **Leaky bucket**: Smooth out traffic spikes

**Why Lua Scripts?**
- **Atomicity**: All operations execute as one unit
- **Performance**: Server-side execution reduces round trips
- **Consistency**: Avoid race conditions in complex logic

### The Challenge
The USS Enterprise (wrong universe, but roll with it) needs to prevent hyperdrive overuse. Too many jumps could tear apart space-time itself!

### Valkey Solution: Lua Scripts + Sorted Sets
```bash
# First, let's create our Lua rate limiting script
# Save this as a script in Valkey (we'll use EVAL for the workshop)

# Sliding window rate limiter - allows 5 jumps per 60 seconds
EVAL "
local key = KEYS[1]
local window = tonumber(ARGV[1])
local limit = tonumber(ARGV[2])  
local current_time = tonumber(ARGV[3])

-- Remove expired entries (outside time window)
redis.call('ZREMRANGEBYSCORE', key, '-inf', current_time - window)

-- Count current requests in window
local current_requests = redis.call('ZCARD', key)

if current_requests < limit then
    -- Add current request with timestamp as score and member
    redis.call('ZADD', key, current_time, current_time)
    redis.call('EXPIRE', key, window)
    return {1, limit - current_requests - 1, current_time + window}
else
    -- Get oldest request time for reset info
    local oldest = redis.call('ZRANGE', key, 0, 0, 'WITHSCORES')
    local reset_time = current_time
    if oldest[2] then
        reset_time = oldest[2] + window
    end
    return {0, 0, reset_time}
end
" 1 hyperdrive:enterprise 60 5 1671234567

# Test multiple requests from Enterprise
EVAL "
local key = KEYS[1]
local window = tonumber(ARGV[1]) 
local limit = tonumber(ARGV[2])
local current_time = tonumber(ARGV[3])

redis.call('ZREMRANGEBYSCORE', key, '-inf', current_time - window)
local current_requests = redis.call('ZCARD', key)

if current_requests < limit then
    redis.call('ZADD', key, current_time, current_time)
    redis.call('EXPIRE', key, window)
    return {1, limit - current_requests - 1, current_time + window}
else
    local oldest = redis.call('ZRANGE', key, 0, 0, 'WITHSCORES')
    local reset_time = current_time
    if oldest[2] then reset_time = oldest[2] + window end
    return {0, 0, reset_time}
end
" 1 hyperdrive:enterprise 60 5 1671234568

# Check current usage without making request
ZCARD hyperdrive:enterprise
ZRANGE hyperdrive:enterprise 0 -1 WITHSCORES
```

### 🚀 Bonus Challenges

**Challenge 1: Multi-Ship Fleet Management**
*"Coordinate the entire Rebel fleet's hyperdrive usage"*

Create rate limiters for multiple ships with different limits
- Commands: Use the script with different keys and limits per ship class

**Challenge 2: Burst Capacity Rate Limiting**
*"Allow emergency hyperdrive bursts for critical missions"*

Implement token bucket algorithm allowing short bursts above normal rate
- Commands: [`EVAL`](https://valkey.io/commands/eval/) with modified Lua logic, [`HINCRBY`](https://valkey.io/commands/hincrby/) for token counts

**Challenge 3: Rate Limit Analytics**
*"Admiral Ackbar needs hyperdrive usage reports"*

Track and analyze rate limit hits and usage patterns
- Commands: [`ZCOUNT`](https://valkey.io/commands/zcount/), [`ZRANGEBYSCORE`](https://valkey.io/commands/zrangebyscore/) for time-based analysis

**Challenge 4: Distributed Rate Limiting**
*"Coordinate limits across multiple starbases"*

Share rate limits across multiple Valkey instances or partitions  
- Commands: [`SCRIPT LOAD`](https://valkey.io/commands/script-load/), [`EVALSHA`](https://valkey.io/commands/evalsha/) for performance

**Challenge 5: Graceful Degradation**
*"When hyperdrive fails, fall back to sublight speeds"*

Implement different rate limits for different service tiers
- Commands: Create tiered rate limiting with multiple keys and fallback logic

---
