# Analysis of Bundesliga player data (SQL)

**Tools:** ClickHouse Cloud, SQL, GitHub.  
**Dataset:** Kaggle: [bundesliga-soccer-player](https://www.kaggle.com/datasets/oles04/bundesliga-soccer-player)

## Tasks completed:

### Task 1. 
Choose the top 3 clubs with the most expensive defense (Defender-*)

```sql
SELECT 
    club, 
    SUM(price) AS total_defense_value
FROM players
WHERE position LIKE 'Defender%'
GROUP BY club
ORDER BY total_defense_value DESC
LIMIT 3;
```

Result:
| club          | total_defense_value |
|---------------|---------------------|
| Bayern Munich | 398.5               |
| RB Leipzig    | 179.5               |
| B. Leverkusen | 145                 |

### Task 2. 
By club and player, count how many players signed a contract with the club after him

```sql
SELECT
    club,
    name,
    joined_club,
    RANK() OVER(PARTITION BY club ORDER BY joined_club DESC) - 1 AS players_after
FROM players;
```

Result (first 10 rows):
| club      | name              | joined_club | players_after |
|-----------|-------------------|-------------|---------------|
| 1.FC Köln | Davie Selke       | 2023-01-02  | 0             |
| 1.FC Köln | Nikola Soldo      | 2022-09-01  | 1             |
| 1.FC Köln | Sargis Adamyan    | 2022-07-05  | 2             |
| 1.FC Köln | Luca Kilian       | 2022-07-01  | 3             |
| 1.FC Köln | Denis Huseinbasic | 2022-07-01  | 3             |
| 1.FC Köln | Kristian Pedersen | 2022-07-01  | 3             |
| 1.FC Köln | Eric Martel       | 2022-07-01  | 3             |
| 1.FC Köln | Florian Dietz     | 2022-07-01  | 3             |
| 1.FC Köln | Linton Maina      | 2022-07-01  | 3             |
| 1.FC Köln | Steffen Tigges    | 2022-07-01  | 3             |
