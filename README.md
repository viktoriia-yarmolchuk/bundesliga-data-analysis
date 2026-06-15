# Analysis of Bundesliga player data (SQL)

**Tools:** ClickHouse Cloud, SQL, GitHub.  
**Dataset:** Kaggle: [bundesliga-soccer-player](https://www.kaggle.com/datasets/oles04/bundesliga-soccer-player)

## Tasks completed:

### Task1. 
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
