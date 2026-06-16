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

### Task 4. 
Choose clubs where the share of Germans is higher than 90%

```sql
SELECT
    club,
    ROUND(AVG(CASE WHEN nationality LIKE '%Germany%' THEN 1 ELSE 0 END) * 100, 2) AS german_players_share
FROM players
GROUP BY club
HAVING german_players_share > 90;
```

Result:
| club            | german_players_share |
|-----------------|----------------------|
| RB Leipzig U19  | 100                  |
| Hertha BSC U19  | 100                  |
| W. Bremen U19   | 100                  |
| FC Augsburg U19 | 100                  |
| W. Bremen II    | 100                  |
| 1.FC Köln U19   | 100                  |
| 1.FC Köln II    | 100                  |
| RB Leipzig U17  | 100                  |
| Hertha BSC II   | 100                  |

### Task 5. 
Choose the most expensive player by each age (name + price)

```sql
SELECT 
    age,
    argMax(name, price) AS most_expensive_player,
    MAX(price) AS max_price
FROM players
GROUP BY age
ORDER BY age;
```

Result (first 10 rows):
| age | most_expensive_player | max_price |
|-----|-----------------------|-----------|
| 17  | Julien Duranville     | 5         |
| 18  | Youssoufa Moukoko     | 30        |
| 19  | Jude Bellingham       | 120       |
| 20  | Jamal Musiala         | 110       |
| 21  | Josko Gvardiol        | 75        |
| 22  | Alphonso Davies       | 70        |
| 23  | Matthijs de Ligt      | 75        |
| 24  | Randal Kolo Muani     | 65        |
| 25  | Christopher Nkunku    | 80        |
| 26  | Kingsley Coman        | 65        |
