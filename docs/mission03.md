## Mission 3: Galactic Task Queues with Lists (12 minutes)
*"I find your lack of queues disturbing..." - Darth Vader, probably*

### 🌟 What are Queues?
Queues are like the mission briefing sequence before a Rebel Alliance strike. Tasks line up in order - first in, first out (FIFO) - ensuring critical missions like destroying Death Stars get proper sequencing. Think of it as organizing Guardians of the Galaxy missions: Star-Lord can't handle all dance-offs simultaneously!

**Why Use Queues?**
- **Asynchronous processing**: Handle tasks without blocking
- **Load balancing**: Distribute work across multiple workers  
- **Reliability**: Ensure tasks aren't lost during system failures
- **Priority handling**: Urgent missions jump to the front

**Queue Patterns:**
- **Producer/Consumer**: Create tasks vs. process tasks
- **Work queues**: Distribute heavy tasks across workers
- **Priority queues**: VIP tasks get handled first
- **Dead letter queues**: Handle failed tasks

### The Challenge
The Guardians of the Galaxy need a mission queue system. Star-Lord can't handle all the dance-offs simultaneously, and Rocket's explosive tasks need proper ordering.

### Valkey Solution: Lists
```bash
# Add missions to standard queue (FIFO - First In, First Out)
RPUSH missions:standard "Deliver mysterious orb to Nova Corps"
RPUSH missions:standard "Collect bounty on Yondu's crew"
RPUSH missions:standard "Fix Milano's navigation system"

# Add urgent missions to front of queue (high priority)
LPUSH missions:urgent "Stop Ronan from destroying Xandar"
LPUSH missions:urgent "Dance-off to save the galaxy"

# Check queue sizes like Gamora checking her weapons
LLEN missions:urgent
LLEN missions:standard  

# Peek at next missions without removing them
LINDEX missions:urgent 0
LRANGE missions:standard 0 2

# Process urgent missions first (FIFO from left)
LPOP missions:urgent

# Process standard missions (FIFO from right/left depending on pattern)
LPOP missions:standard

# View all remaining missions
LRANGE missions:urgent 0 -1
LRANGE missions:standard 0 -1
```

### 🚀 Bonus Challenges

**Challenge 1: Mission Priority Reordering**
*"I am Groot" - Task prioritization*

Move a standard mission to urgent queue and insert at specific position
- Commands: [`LREM`](https://valkey.io/commands/lrem/), [`LINSERT`](https://valkey.io/commands/linsert/), [`LSET`](https://valkey.io/commands/lset/)

**Challenge 2: Batch Mission Processing**  
*"We are Groot" - Batch operations*

Process multiple missions at once and handle empty queues gracefully
- Commands: [`LPOP`](https://valkey.io/commands/lpop/) with count, [`BRPOP`](https://valkey.io/commands/brpop/) for blocking pops

**Challenge 3: Mission Queue Monitoring**
*"Rocket's mission queue dashboard"*

Get queue statistics and find specific missions without removing them
- Commands: [`LPOS`](https://valkey.io/commands/lpos/), [`LTRIM`](https://valkey.io/commands/ltrim/) for queue size limits

**Challenge 4: Reliable Task Processing**
*"Groot's backup mission system"*

Implement reliable processing where failed missions can be retried
- Commands: [`RPOPLPUSH`](https://valkey.io/commands/rpoplpush/), [`BRPOPLPUSH`](https://valkey.io/commands/brpoplpush/)

---