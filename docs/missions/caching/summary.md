[Previous](../docs/missions.md) | [Homepage](../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../README.md)

![Keyspace](../static/img/keyspace-backdrop.png)

[Mission 1: Hyperdrive Caching with Strings](../caching/README.md)

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
