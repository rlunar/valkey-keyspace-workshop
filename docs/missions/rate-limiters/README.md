[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

# Mission 5️⃣: Hyperdrive Rate Limiters with Lua Scripts

*"The hyperdrive is damaged, Captain. We need to limit our jumps!"*

![Kessel-run](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbGpmbTc4dzBmMnloOHVyc3d4M3k1ZXgzcXcwaXJueXhqajJwejl5bCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/xTiIzxt2LbvM1mi2nS/giphy.gif)

## 🌟 What is Rate Limiting?

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
