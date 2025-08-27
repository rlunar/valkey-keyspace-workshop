[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 2️⃣: Cantina Session Management with Hashes__

# 🚀 Bonus Challenges Solutions

## **Challenge 1: Session Analytics**

*"How many customers are currently at the Mos Eisley Cantina with an open tab?"*

Objective: Count total active sessions and get all session keys only of the hash type

__SetUp__

```bash
HSET session:chewbacca username "chewbacca" species "wookie" credits 5000 table "booth_7" drinks_ordered "7" last_activity "2025-08-28T13:23:58"
```

Response:
> (integer) 6

```bash
HSET session:han_solo username "hsolo" species "human" credits 10000 table "booth_7" drinks_ordered "2" last_activity "2025-08-28T13:30:00"
```

Response:
> (integer) 6

```bash
HSET session:luke_skywalker username "lskywalker" species "human" credits 100 table "bar_3" drinks_ordered "1" last_activity "2025-08-28T13:33:12"
```

Response:
> (integer) 6

Hint Commands: [`SCAN`](https://valkey.io/commands/scan/) with patterns, [`DBSIZE`](https://valkey.io/commands/dbsize/)

__Solution__

```bash
SCAN 0 MATCH session:* COUNT 100 TYPE hash
```

Response:
>
> 1) "0"
> 2) 1) "session:han_solo"
>    2) "session:chewbacca"
>    3) "session:luke_skywalker"
>

## **Challenge 2: Session Field Operations**

*"The Force is strong with this session field manipulation"*

Increment drink orders atomically and handle numeric operations.

Objective: Increment by 1 Han Solo's drinks and Chewbacca's by 3.

Use same customer sessions from above.

Hint Commands: [`HINCRBY`](https://valkey.io/commands/hincrby/), [`HINCRBYFLOAT`](https://valkey.io/commands/hincrbyfloat/)

__Solution__

```bash
HINCRBY session:han_solo drinks_ordered 1
```

Response:
> (integer) 3

```bash
HINCRBY session:chewbacca drinks_ordered 3
```

Response:
> (integer) 10

_Do not let that Wookie pilot tonight!_

## **Challenge 3: Session Validation**

*"These aren't the session fields you're looking for"*

Get only the fields that exist and count total session fields

Objectives:
- Count all key/value pairs on Luke's session
- Fetch all fields (keys) from Han's session
- Get all values from Chewbacca's session

Hint Commands: [`HLEN`](https://valkey.io/commands/hlen/), [`HKEYS`](https://valkey.io/commands/hkeys/), [`HVALS`](https://valkey.io/commands/hvals/)

__Solution__

```bash
HLEN session:luke_skywalker
```

Response:
> (integer) 6

```bash
HKEYS session:han_solo
```

Response:
>
> 1) "username"
> 2) "species"
> 3) "credits"
> 4) "table"
> 5) "drinks_ordered"
> 6) "last_activity"
>

```bash
HVALS session:chewbacca
```

Response:
>
> 1) "chewbacca"
> 2) "wookie"
> 3) "5000"
> 4) "booth_7"
> 5) "10"
> 6) "2025-08-28T13:23:58"
>

## ➡️ Next: [Mission 3️⃣: Galactic Task Queues with Lists](../queues/README.md)
