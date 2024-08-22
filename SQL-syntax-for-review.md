1. ``` DATE_PART('day', tweet_date)` AS tweet_day ```
2. ``` RANK() OVER (PARTITION BY user_id ORDER BY tweet_count) AS user_tweet_no ```
3. ``` ROUND( AVG(tweet_count) OVER( PARTITION BY user_id, ORDER BY tweet_date ROWS 2 PRECEEDING CURRENT ROW ) , 2)  AS tweet_avg_3day ```
4. 
```
-- Sample CASE and cte usage (not correct approach for mobile_views) 
SELECT user_id,
       CASE device_type 
          WHEN 'tablet' THEN 'mobile'
          WHEN 'phone' THEN 'mobile'
          ELSE 'tablet'
      END AS device_type,
      view_time
  FROM viewership
)
```
5.
```
-- Sample Usage of FILTER WHERE()
SELECT 
COUNT(*) FILTER(WHERE device_type = 'laptop') AS laptop_views,
COUNT(*) FILTER(WHERE device_type = 'tablet' OR device_type = 'phone' ) AS mobile_views    
FROM viewership;
-- GROUP BY device_type (not required here, as FILTER operation)
```

6.
```
-- Subtracting Dates, in SQL Server DATEDIFF(day, start_date, end_date) also works

SELECT user_id, 
      DATE_PART('day', (last_post_date - first_post_date)) AS days_between              -- otherwise o/p is in Days Hours
FROM cte_2
WHERE DATE_PART('day', (last_post_date - first_post_date)) != 0                           -- HAVING cannot be used here as it is used along in aggregations NOT otherwise
```
7.
```
-- Sample Code for JOIN, using IN (not **[** rather **(**) 
with cte_1 AS (
SELECT
      dept.department_name,
      emp.name,
      emp.salary,
  DENSE_RANK() OVER(PARTITION BY dept.department_name ORDER BY emp.salary DESC) AS dept_sal_rank      
FROM employee AS emp
LEFT JOIN department AS dept ON emp.department_id = dept.department_id
)

SELECT department_name, salary, name
FROM cte_1
WHERE dept_sal_rank in (1,2,3)
ORDER BY department_name, salary DESC
;

```


