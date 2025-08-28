[Previous](../docs/missions.md) | [Homepage](../../../README.md) | [Valkey Keyspace Workshop: A Galactic Guide to Common Use Cases](../../../README.md)

![Keyspace](../../../static/img/keyspace-backdrop.png)

# Mission 4️⃣: Death Star Leaderboards with Sorted Sets

![Death-Star](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExazkxNWdtczJ3c3NpbjRlcXN3bjZxbXF6ZHBhNTdiOXN2ZHg3NmVuNiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1xo9COytfPE9chS05b/giphy.gif)

## 🌟 What are Leaderboards?

Leaderboards are like the Imperial Academy's pilot rankings - automatically sorted lists where performance determines position. Whether tracking TIE fighter kill counts or Podracing lap times, sorted sets maintain real-time rankings without manual sorting.

**Why Use Leaderboards?**

- **Gamification**: Drive engagement through competition
- **Performance tracking**: Monitor metrics in real-time
- **Automatic sorting**: No manual rank calculations needed
- **Range queries**: Find top performers or specific score ranges
- **Efficient updates**: O(log N) insertions and updates

**Leaderboard Applications:**

- **Gaming**: Player scores and achievements  
- **E-commerce**: Product ratings and reviews
- **Analytics**: Performance metrics and KPIs
- **Social**: User reputation and activity scores

### The Challenge

The Empire needs to rank TIE fighter pilots by their hit accuracy. Even storm troopers deserve recognition (when they actually hit something).

### Solution: Valkey Sorted Sets

1. Add pilots with accuracy scores (score is the ranking value)

```bash
ZADD pilot_leaderboard 98.5 "Darth_Vader"
```

Response:
> (integer) 1

```bash
ZADD pilot_leaderboard 99.9 "Luke_Skywalker"  
```

Response:
> (integer) 1

```bash
ZADD pilot_leaderboard 87.3 "Wedge_Antilles"
```

Response:
> (integer) 1

You can also add multiple items at once:

```bash
ZADD pilot_leaderboard 12.3 "Stormtrooper_TK421" 15.7 "Stormtrooper_TK422" 94.2 "Biggs_Darklighter"
```

2. Get top pilots (highest scores first)

```bash
ZREVRANGE pilot_leaderboard 0 4 WITHSCORES
```

Response:
>
>  1) "Luke_Skywalker"
>  2) "99.9"
>  3) "Darth_Vader"
>  4) "98.5"
>  5) "Biggs_Darklighter"
>  6) "94.2"
>  7) "Wedge_Antilles"
>  8) "87.3"
>  9) "Stormtrooper_TK422"
> 10) "15.7"
>

3. Get bottom performers (lowest scores)

![Stormtrooper_TK421](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExajZlNjBkYzBjcHJlYmI1M3N1a2pyZmo4bzNrZGEzaXNsaHg2ZWI5NCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/DtrguqHYeHJv2/giphy.gif)

```bash
ZRANGE pilot_leaderboard 0 2 WITHSCORES
```

Response:
>
1) "Stormtrooper_TK421"
2) "12.3"
3) "Stormtrooper_TK422"
4) "15.7"
5) "Wedge_Antilles"
6) "87.3"
>

4. Find specific pilot's rank (0-based, highest score = rank 0)

```bash
ZREVRANK pilot_leaderboard "Luke_Skywalker"
```

Response:
> (integer) 0

```bash
ZRANK pilot_leaderboard "Stormtrooper_TK421"
```

Response:
> (integer) 0

5. Get pilot's score

```bash
ZSCORE pilot_leaderboard "Darth_Vader"
```

Response:
> "98.5"

6. Update scores (pilot improvement after training)

```bash
ZADD pilot_leaderboard 45.6 "Stormtrooper_TK421"
```

Response:
> (integer) 0

7. Get pilots within accuracy range (elite squadron criteria)

```bash
ZRANGEBYSCORE pilot_leaderboard 90 100 WITHSCORES
```

Response:
>
> 1) "Biggs_Darklighter"
> 2) "94.2"
> 3) "Darth_Vader"
> 4) "98.5"
> 5) "Luke_Skywalker"
> 6) "99.9"
>

8. Count pilots in score range

```bash
ZCOUNT pilot_leaderboard 90 100
```

Response:
> (integer) 3

## ➡️ Next: [Test your skills with challenges](../leaderboards/challenge.md)
