```sql
--1. If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?
SELECT SUM(CASE WHEN pizza_id = 1 THEN 12
		WHEN pizza_id = 2 THEN 10
           END) AS money_made
FROM customer_orders c
JOIN runner_orders r
ON c.order_id = r.order_id
WHERE cancellation IS NULL;

--2 What if there was an additional $1 charge for any pizza extras? Add cheese is $1 extra
WITH price1_CTE AS (
	SELECT SUM(CASE WHEN pizza_id = 1 THEN 12 ELSE 10 END) AS 'money_made'
	FROM customer_orders c
	JOIN runner_orders r
	ON c.order_id = r.order_id
	WHERE cancellation IS NULL
),
price2_CTE AS (
	SELECT SUM(CASE WHEN extras = 4 THEN 2 ELSE 1 END) AS 'extras_charged'
	FROM customer_orders c
	JOIN runner_orders r
	ON c.order_id = r.order_id
	WHERE cancellation IS NULL
    AND extras IS NOT NULL
)
SELECT (money_made + extras_charged) AS adjusted_money_made
FROM price1_CTE, price2_CTE;

--3 The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.
DROP TABLE IF EXISTS runner_ratings;
CREATE TABLE runner_ratings (
  order_id INT,
  rating INT
);
INSERT INTO runner_ratings (order_id, rating)
VALUES 
  (1,5),
  (2,3),
  (3,2),
  (4,4),
  (5,2),
  (7,3),
  (8,4),
  (10,5);

SELECT * FROM runner_ratings;

--4 Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?customer_id, order_id, runner_id, rating, order_time, pickup_time,Time between order and pickup, Delivery duration, Average speed, Total number of pizzas
SELECT
    co.customer_id,
    co.order_id,
    ro.runner_id,
    rr.rating,
    co.order_time,
    ro.pickup_time,
    CAST(DATEADD(MINUTE, DATEDIFF(MINUTE, co.order_time, ro.pickup_time), '1900-01-01') AS TIME) AS time_btwn_order_and_pickup,
    ro.duration_min,
    CEILING(AVG(distance_km / (ro.duration_min / 60.0))) AS avg_speed_km_hr,
    COUNT(co.pizza_id) AS num_pizzas
FROM
    customer_orders AS co
JOIN
    runner_orders AS ro ON co.order_id = ro.order_id
JOIN
    runner_ratings AS rr ON rr.order_id = ro.order_id
GROUP BY
    co.customer_id,
    co.order_id,
    ro.runner_id,
    rr.rating,
    co.order_time,
    ro.pickup_time,
    ro.duration_min;

--5 If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries?
WITH income_CTE AS (
	SELECT SUM(CASE WHEN c.pizza_id = 1 THEN 12 ELSE 10 END) AS money_made
	 FROM customer_orders c
	 INNER JOIN pizza_names pn
	 ON c.pizza_id = pn.pizza_id
	 INNER JOIN runner_orders r
	 ON c.order_id = r.order_id
	 WHERE r.cancellation IS NULL
     ),
expense_CTE AS(
SELECT SUM((distance_km * 0.30)) AS total_driver_fee
FROM runner_orders
WHERE distance_km IS NOT NULL
)
SELECT ROUND(money_made - total_driver_fee, 2) AS leftover_amt
FROM income_CTE, expense_CTE;
