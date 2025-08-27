[Previous](../caching/challenge.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 1️⃣: Hyperdrive Caching with Strings__

# 🎯 Cache Strategy Selection Guide

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

## ➡️ Next: [Mission 2️⃣: Cantina Session Management with Hashes](../docs/mission02.md)
