[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

# Mission 3️⃣: Galactic Task Queues with Lists

## 🌟 What are Queues?

Queues are like the mission briefing sequence before a Rebel Alliance strike. Tasks line up in order - first in, first out (FIFO) - ensuring critical missions like destroying Death Stars get proper sequencing. Think of it as organizing Guardians of the Galaxy missions: Star-Lord can't handle all dance-offs simultaneously!

![Starlord-dance](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExajhibDdkZ2dkazlmdmFxOXRqbXAzZGtzOXppdmZ5aDA3bjQ2ZWdhcSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/wv34jEfv17OmI/giphy.gif)

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

### Solution: Valkey Lists

1. Add missions to standard queue (FIFO - First In, First Out)

```bash
RPUSH missions:standard "Deliver mysterious orb to Nova Corps"
```

Response:
> (integer) 1

```bash
RPUSH missions:standard "Collect bounty on Yondu's crew"
```

Response:
> (integer) 2

```bash
RPUSH missions:standard "Fix Milano's navigation system"
```

Response:
> (integer) 3

As you can see, every time we add a new item to the missions queue, we get back the total number of elements in the list.

2. Add urgent missions to front of queue (high priority)

```bash
LPUSH missions:urgent "Stop Ronan from destroying Xandar"
```

Response:
> (integer) 1

```bash
LPUSH missions:urgent "Dance-off to save the galaxy"
```

Response:
> (integer) 2

3. Check queue sizes like Gamora checking her weapons

```bash
LLEN missions:urgent
```

Response:
> (integer) 2

```bash
LLEN missions:standard
```

Response:
> (integer) 3

4. Peek at next missions without removing them

```bash
LINDEX missions:urgent 0
```

Response:
> "Dance-off to save the galaxy"

```bash
LRANGE missions:standard 0 2
```

Response:
>
> 1) "Deliver mysterious orb to Nova Corps"
> 2) "Collect bounty on Yondu's crew"
> 3) "Fix Milano's navigation system"
>

5. Process urgent missions first (FIFO from left)

```bash
LPOP missions:urgent
```

Response:
> "Dance-off to save the galaxy"

As you can see this is the same as item 0 as it is the Leftmost item in the list.

6. Process standard missions (FIFO from right/left depending on pattern)

```bash
LPOP missions:standard
```

Response:
> "Deliver mysterious orb to Nova Corps"

7. View all remaining missions

```bash
LRANGE missions:urgent 0 -1
```

Response:
> 1) "Stop Ronan from destroying Xandar"

```bash
LRANGE missions:standard 0 -1
```

## ➡️ Next: [Test your skills with challenges](../queues/challenge.md)
