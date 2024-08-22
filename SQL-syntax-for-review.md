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
5. ```
   -- Sample Usage of FILTER WHERE()
   SELECT 
    COUNT(*) FILTER(WHERE device_type = 'laptop') AS laptop_views,
    COUNT(*) FILTER(WHERE device_type = 'tablet' OR device_type = 'phone' ) AS mobile_views    
    FROM viewership;
    -- GROUP BY device_type (not required here, as FILTER operation)
   ```
