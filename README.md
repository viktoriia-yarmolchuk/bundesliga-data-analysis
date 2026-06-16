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
    $RANK() OVER(PARTITION BY club ORDER BY joined_club DESC) - 1 AS players_after
FROM players;
```
***RANK()** was used instead of **ROW_NUMBER()** to correctly count players who joined on the same day*

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

### Task 3. 
Choose clubs where the average value of French players is more than 5 million

```sql
SELECT
    club,
    ROUND(AVG(price), 2) AS avg_value
FROM players
WHERE nationality LIKE '%France%'
GROUP BY club
HAVING avg_value > 5;
```

Result:
| club            | avg_value |
|-----------------|-----------|
| SC Freiburg     | 6.6       |
| Bor. M'gladbach | 15.42     |
| Bayern Munich   | 43.21     |
| Hertha BSC      | 5.07      |
| B. Leverkusen   | 32.5      |
| TSG Hoffenheim  | 8.5       |
| 1.FC Köln       | 8.25      |
| 1.FSV Mainz 05  | 8.75      |
| Bor. Dortmund   | 13        |
| VfL Wolfsburg   | 9.33      |
| RB Leipzig      | 41        |
| E. Frankfurt    | 26.75     |
| Union Berlin    | 5.33      |
