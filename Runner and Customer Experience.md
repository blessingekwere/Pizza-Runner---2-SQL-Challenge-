```sql
--1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
SELECT 
	CASE WHEN registration_date BETWEEN '2021-01-01' AND '2021-01-07' THEN 'Week 1'
	ELSE 'Week 2' END AS Week, COUNT(*) AS sign_ups
FROM runners
GROUP BY CASE WHEN registration_date BETWEEN '2021-01-01' AND '2021-01-07' THEN 'Week 1'
	ELSE 'Week 2' END
ORDER BY Week;

--2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
SELECT ro.runner_id, 
       AVG(DATEDIFF(MINUTE, co.order_time, ro.pickup_time)) AS avg_time
FROM runner_orders AS ro
JOIN customer_orders AS co
ON ro.order_id = co.order_id
WHERE ro.pickup_time <> 'null'
GROUP BY ro.runner_id;

-- 3. Is there any relationship between the number of pizzas and how long the order takes to prepare?
SELECT no_of_Pizzas, prep_duration
FROM (Select c.order_id, Count(c.pizza_id) AS no_of_Pizzas, DATEDIFF(MINUTE, c.order_time, r.pickup_time) AS prep_duration
	FROM customer_orders c
	JOIN runner_orders r
	ON c.order_id = r.order_id
	Where r.pickup_time IS NOT NULL
	GROUP BY  c.order_id, c.order_time, r.pickup_time) AS subquery
Order By no_of_Pizzas

--4. What was the average distance travelled for each customer?
SELECT c.customer_id, ROUND(AVG(r.distance_km),2) AS avg_distance
FROM runner_orders r
JOIN customer_orders c
ON r.order_id = c.order_id
WHERE r.distance_km IS NOT NULL
GROUP BY c.customer_id

--5. What was the difference between the longest and shortest delivery times for all orders?
SELECT MAX(duration_min) - MIN(duration_min) AS duration_difference
FROM runner_orders
WHERE duration_min IS NOT NULL

--6. What was the average speed for each runner for each delivery and do you notice any trend for these values?
SELECT runner_id, ROUND(AVG(distance_km / (duration_min / 60.0)), 2) AS avg_speed
FROM runner_orders
WHERE distance_km IS NOT NULL AND duration_min IS NOT NULL
GROUP BY runner_id;

--7. What is the successful delivery percentage for each runner?
SELECT runner_id,
       SUM(CASE WHEN cancellation IS NULL THEN 1 ELSE 0 END) AS successful_delivery,
       CAST(ROUND((SUM(CASE WHEN cancellation IS NULL THEN 1 ELSE 0 END) * 100.0) / COUNT(*), 2)AS INT) AS success_percentage
FROM runner_orders
