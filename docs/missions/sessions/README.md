[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

# Mission 2️⃣: Cantina Session Management with Hashes (10 minutes)

## 🌟 What are Sessions?
Sessions are like your cantina tab - keeping track of who you are, what you've ordered, and how much you owe. In web applications, sessions maintain user state across multiple requests, like remembering Han Solo still owes money for his drinks.

![Han-Solo-Mos-Eisley-Cantina](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ2k2cmY3MWdrbmd1YmdzcDY1MzQ1OHB0NDFvOHAzaDJhdXE2Y3U2eiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/xTiIzET0onuPUotXjy/giphy.gif)

**Why Use Sessions?**
- **Stateful web apps**: HTTP is stateless, sessions add memory
- **User authentication**: Remember logged-in users
- **Shopping carts**: Persist user selections
- **Personalization**: Store user preferences and history

**Session Storage Requirements:**
- **Fast access**: Users don't wait for slow session lookups  
- **Structured data**: More than just simple strings
- **Expiration**: Automatic cleanup of old sessions
- **Partial updates**: Modify individual session fields

### The Challenge
Mos Eisley Cantina needs to track customer sessions across multiple tables. Even Greedo deserves a good user experience (before Han shoots first).

### Solution: [Valkey Hashes](https://valkey.io/commands/#hash)

Let's say we want to keep track of each tab for everyone at the cantina.

Our data model looks like a Python Dictionary, Java Hashmap, PHP Array or JavaScript Object:

```python
han_solo = {
    "username": "hsolo",
    "species": "human",
    "credits": 10000,
    "table": "booth_7",
    "drinks_ordered": 2,
    "last_activity": "2025-08-28T13:30:00"
}
```

__1. Create comprehensive user session__
```bash
HSET session:han_solo username "hsolo" species "human" credits 10000 table "booth_7" drinks_ordered "2" last_activity "2025-08-28T13:30:00"
```

We get a response to the number of field/value pairs that we have stored in our Valkey Hash.
> (integer) 6

__2. Set session expiration (1 hour)__

```bash
EXPIRE session:han_solo 3600
```

Response:
> (integer) 1

__3. Access specific session data faster than Jedi reflexes__

```bash
HGET session:han_solo credits
```

Response:
> "10000"

```bash
HGET session:han_solo table
```

Response:
> "booth_7"

__4. Update credits after paying for drinks (partial update)__

```bash
HSET session:han_solo credits 9500 last_activity "2022-08-28T13:45:00"
```

Response, no new fields were added to the Valkey Hash:
> (integer) 0

__5. Get all session data at once__

```bash
HGETALL session:han_solo
```

Response:
>
>  1) "username"
>  2) "hsolo"
>  3) "species"
>  4) "human"
>  5) "credits"
>  6) "9500"
>  7) "table"
>  8) "booth_7"
>  9) "drinks_ordered"
> 10) "2"
> 11) "last_activity"
> 12) "2024-12-10T15:45:00"
>

__6. Check if session field exists__

```bash
HEXISTS session:han_solo bounty_on_head
```

Response, when the field does not exist we get `0` otherwise `1`:
> (integer) 0

__7. Get multiple fields efficiently__

```bash
HMGET session:han_solo username credits species
```

Response:
>
> 1) "hsolo"
> 2) "9500"
> 3) "human"
>

## ➡️ Next: [Test your skills with challenges](../sessions/challenge.md)
