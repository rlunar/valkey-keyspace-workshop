[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

__Mission 2️⃣: Cantina Session Management with Hashes__

# 🚀 Bonus Challenges [Optional]

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

## **Challenge 2: Session Field Operations**

*"The Force is strong with this session field manipulation"*

Increment drink orders atomically and handle numeric operations.

Objective: Increment by 1 Han Solo's drinks and Chewbacca's by 3.

Use same customer sessions from above.

Hint Commands: [`HINCRBY`](https://valkey.io/commands/hincrby/), [`HINCRBYFLOAT`](https://valkey.io/commands/hincrbyfloat/)

## **Challenge 3: Session Validation**

*"These aren't the session fields you're looking for"*

Get only the fields that exist and count total session fields

Objectives:
- Count all key/value pairs on Luke's session
- Fetch all fields (keys) from Han's session
- Get all values from Chewbacca's session

Hint Commands: [`HLEN`](https://valkey.io/commands/hlen/), [`HKEYS`](https://valkey.io/commands/hkeys/), [`HVALS`](https://valkey.io/commands/hvals/)

## ➡️ Next: [Mission 3️⃣: Galactic Task Queues with Lists](../queues/README.md)

[Mission 2️⃣ Challenge Solutions](../sessions/solution.md)
