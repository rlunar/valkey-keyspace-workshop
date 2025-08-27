[Previous](../caching/challenge.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 1️⃣: Hyperdrive Caching with Strings__

# 🚀 Bonus Challenges [Optional]

## **Challenge 1: Cache Invalidation Pattern**

*"Your lack of cache invalidation disturbs me" - Vader*

Objective: Use pattern matching to clear all Imperial fleet caches at once

__SetUp__

```bash
SET imperial_fleet:tatooine "3 Star Destroyers, sector 7G"
```

Response:
> OK

Cache multiple systems 📋

```bash
SET imperial_fleet:hoth "1 Super Star Destroyer, 6 Star Destroyers"
```

Response:
> OK

Cache multiple systems 📋

```bash
SET imperial_fleet:endor "Shield generator station, 2 Star Destroyers"
```

Response:
> OK

Validation: All following commands should return `(nil)`:

```bash
GET imperial_fleet:tatooine
```

```bash
GET imperial_fleet:hoth
```

```bash
GET imperial_fleet:endor
```

Hint Commands: [`SCAN`](https://valkey.io/commands/scan/), [`UNLINK`](https://valkey.io/commands/unlink/)

__Solution__

Find all the Keys that are about the Imperial Fleet:

```bash
SCAN 0 MATCH imperial_fleet:* COUNT 10
```

Response:
>
> 1) "0"
> 2) 1) "imperial_fleet:tatooine"
>    2) "imperial_fleet:endor"
>    3) "imperial_fleet:hoth"
>    4) "imperial_fleet:hoth:atat"
>

Remove the Keys we don not need anymore:

```bash
UNLINK imperial_fleet:tatooine imperial_fleet:endor imperial_fleet:hoth
```

Response:
> (integer) 3

## **Challenge 2: Atomic Updates**

*"I find your race condition disturbing"*

Objective: Update fleet strength only if the current value matches expected value

SetUp

```bash
SET imperial_fleet:hoth:atat 7 EX 600
```

Response:
> OK

Now, after the Rogue Squadron took down 2 of them we need to update the value but verify if its the previous value (7), we don't want to misscount how many more AT-AT are still out there destroying power generators.

Hint Commands: [`GET`](https://valkey.io/commands/get/), [`SET`](https://valkey.io/commands/set/) with conditions

__Solution__

```bash
SET imperial_fleet:hoth:atat 5 IFEQ 7 EX 600
```

Response:
> OK

If we retrieve the data we should have 5 as response 📋

```bash
GET imperial_fleet:hoth:atat
```

Response:
> "5"

## **Challenge 3: Bulk Operations**

*"These are not the slow operations you're looking for"*

Objective: Set multiple fleet positions in a single atomic operation, instead of doing 3 separate commands to store or retrieve, how would you do it at once?

Original:

```bash
SET imperial_fleet:tatooine "3 Star Destroyers, sector 7G"
```

Response:
> OK

Cache multiple systems 📋

```bash
SET imperial_fleet:hoth "1 Super Star Destroyer, 6 Star Destroyers"
```

Response:
> OK

Cache multiple systems 📋

```bash
SET imperial_fleet:endor "Shield generator station, 2 Star Destroyers"
```

Response:
> OK

Hint Commands: [`MSET`](https://valkey.io/commands/mset/), [`MGET`](https://valkey.io/commands/mget/)

__Solution__

```bash
MSET imperial_fleet:tatooine "3 Star Destroyers, sector 7G" imperial_fleet:hoth "1 Super Star Destroyer, 6 Star Destroyers" imperial_fleet:endor "Shield generator station, 2 Star Destroyers"
```

Response:
> OK

Similarly we can retrieve all 3 keys at once:

```bash
MGET imperial_fleet:tatooine imperial_fleet:hoth imperial_fleet:endor
```

Response:

>
> 1) "3 Star Destroyers, sector 7G"
> 2) "1 Super Star Destroyer, 6 Star Destroyers"
> 3) "Shield generator station, 2 Star Destroyers"
>

## **Challenge 4: Conditional Caching**

*"Only store this intelligence if no one else has reported it"*

Objective: Cache fleet data only if the key doesn't exist yet

You can always do the following commands, but multiple Pilots might be reporting and overwriting the same information:

Pilot Red Five reports 📋

```bash
SET imperial_fleet:endor "1 Super Star Destroyer, 6 Star Destroyers" EX 600
```

Response:
> OK

Pilot Rogue Two also reports 📋

```bash
SET imperial_fleet:endor "1 Super Star Destroyer, 6 Star Destroyers" EX 600
```

Response:
> OK

How can you prevent this?

Hint Commands: [`SET`](https://valkey.io/commands/set/) with `NX` option, [`SETNX`](https://valkey.io/commands/setnx/)

__Solution__

```bash
SET imperial_fleet:endor "1 Super Star Destroyer, 6 Star Destroyers" EX 600 NX
```

Response:
> (nil)

If we try to get the data still there

```bash
GET imperial_fleet:endor
```

Response:
> "1 Super Star Destroyer, 6 Star Destroyers"

## ➡️ Next: [Mission 2️⃣: Cantina Session Management with Hashes](../sessions/README.md)
