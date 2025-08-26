[Previous](../docs/missions.md) | [Homepage](../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../README.md)

![Keyspace](../static/img/keyspace-backdrop.png)

# Mission 1: Hyperdrive Caching with Strings (16 minutes)



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
