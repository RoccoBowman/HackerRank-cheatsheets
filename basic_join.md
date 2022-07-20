## Contest Leaderboard
-Medium

```sql
SELECT
    hacker_id,
    name,
    SUM(max) AS total_score -- We will add the top scores per hacker and challenge
FROM(SELECT -- Similar to the subquery in the new company problem; join hackers to submissions
        h.name,
        h.hacker_id,
        s.challenge_id,
        MAX(score) AS max -- find the max score for each submission
     FROM Hackers AS h
     LEFT JOIN Submissions AS s ON (h.hacker_id = s.hacker_id)
     GROUP BY h.name, h.hacker_id, s.challenge_id) AS subquery -- finish the subquery by grouping by hacker name, id, and challege
                                                               -- this leaves us with all the top scores per hacker, per challenge
GROUP BY hacker_id, name -- Then we just have to group by hacker_id, and name, the sum of top scores per hacker and challenge is appended
HAVING SUM(max) > 0
ORDER BY total_score DESC, hacker_id ASC;
```
