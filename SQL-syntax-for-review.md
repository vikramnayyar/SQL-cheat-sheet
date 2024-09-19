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

8.
```
-- Sample Code for single MAX(CASE WHEN ) - pivoting using CASE WHEN

with engg_cte as (
select e.salary as salary,
        d.department as dept
from db_employee AS e
left join db_dept AS d on e.department_id = d.id
WHERE d.department in ('engineering', 'marketing')
)

select ABS(MAX(case WHEN dept = 'engineering' then salary else null end) - MAX(case WHEN dept = 'marketing' then salary else null end)) as salary_difference
from engg_cte
;

```

9. Usage of SELECT FILTER (Always Check Before Join) & FORMULA in SELECT
```

-- select * from facebook_posts;

with cte_1 as (
    SELECT post_date,
            count(post_id) as total_spam
    FROM facebook_posts
    WHERE post_id in (SELECT post_id FROM facebook_post_views) and post_keywords like "%spam%" 
    GROUP BY post_date
)
,

cte_2 as (
    SELECT post_date,
            count(post_id) as total_posts
    FROM facebook_posts
    WHERE post_id in (SELECT post_id FROM facebook_post_views)
    GROUP BY post_date
),

cte_3 as (
    SELECT t.post_date, 
            (((s.total_spam) / t.total_posts)*100) as spam_share
            from cte_2 as t
    left join cte_1 as s on s.post_date = t.post_date 
            
)

select * from cte_3
;
```



DISCOUNT COUPON STRATASCRATCH - BUILDINGBLOCKS30 (30% off on yearly membership)


