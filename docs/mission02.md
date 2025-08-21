## Mission 2: Cantina Session Management with Hashes (12 minutes)
*"These aren't the sessions you're looking for..."*

### 🌟 What are Sessions?
Sessions are like your cantina tab - keeping track of who you are, what you've ordered, and how much you owe. In web applications, sessions maintain user state across multiple requests, like remembering Han Solo still owes money for his drinks.

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

### Valkey Solution: Hashes
```bash
# Create comprehensive user session
HSET session:han_solo username "hsolo" species "human" credits 10000 table "booth_7" drinks_ordered "2" last_activity "2024-12-10T15:30:00"

# Set session expiration (1 hour)  
EXPIRE session:han_solo 3600

# Access specific session data faster than Jedi reflexes
HGET session:han_solo credits
HGET session:han_solo table

# Update credits after paying for drinks (partial update)
HSET session:han_solo credits 9500 last_activity "2024-12-10T15:45:00"

# Get all session data at once
HGETALL session:han_solo

# Check if session field exists
HEXISTS session:han_solo bounty_on_head

# Get multiple fields efficiently
HMGET session:han_solo username credits species
```

### 🚀 Bonus Challenges

**Challenge 1: Session Analytics**
*"How many Bothans died to bring us this session information?"*

Count total active sessions and get all session keys
- Commands: [`KEYS`](https://valkey.io/commands/keys/) with patterns, [`DBSIZE`](https://valkey.io/commands/dbsize/)

**Challenge 2: Session Field Operations**
*"The Force is strong with this session field manipulation"*

Increment drink orders atomically and handle numeric operations
- Commands: [`HINCRBY`](https://valkey.io/commands/hincrby/), [`HINCRBYFLOAT`](https://valkey.io/commands/hincrbyfloat/)

**Challenge 3: Session Validation**
*"These aren't the session fields you're looking for"*

Get only the fields that exist and count total session fields  
- Commands: [`HLEN`](https://valkey.io/commands/hlen/), [`HKEYS`](https://valkey.io/commands/hkeys/), [`HVALS`](https://valkey.io/commands/hvals/)

**Challenge 4: Bulk Session Management**
*"Execute Order 66... I mean, delete expired sessions"*

Create multiple sessions and manage them efficiently
- Commands: [`HMSET`](https://valkey.io/commands/hmset/), [`HDEL`](https://valkey.io/commands/hdel/), [`SCAN`](https://valkey.io/commands/scan/)

---
