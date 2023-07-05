``` sql
--1. How many pizzas were ordered?
SELECT COUNT(order_id) as total_pizza
FROM customer_orders;


--2. How many unique customer orders were made?
SELECT COUNT(DISTINCT order_id) AS unique_orders
FROM customer_orders

--3. How many successful orders were delivered by each runner?
SELECT runner_id, COUNT(order_id) AS orders_delivered
FROM runner_orders
WHERE cancellation IS NULL
GROUP BY runner_id;

-- 4. How many of each type of pizza was delivered?
SELECT pizza_id, COUNT(pizza_id) AS num_delivered
FROM customer_orders AS co
JOIN runner_orders AS ro 
ON co.order_id = ro.order_id
WHERE cancellation IS NULL
GROUP BY pizza_id;

--5. How many Vegetarian and Meatlovers were ordered by each customer?
SELECT co.customer_id, 
       COUNT(CASE WHEN CONVERT(varchar(255), pn.pizza_name) = 'Vegetarian' THEN 1 ELSE NULL END) AS vegetarian,
       COUNT(CASE WHEN CONVERT(varchar(255), pn.pizza_name) = 'Meatlovers' THEN 1 ELSE NULL END) AS meatlovers
FROM customer_orders co
JOIN pizza_names pn 
ON pn.pizza_id = co.pizza_id
GROUP BY co.customer_id;

-- 6. What was the maximum number of pizzas delivered in a single order?
SELECT TOP 1 co.order_id, COUNT(pizza_id) as pizza_delivered
FROM customer_orders co
JOIN runner_orders ro
ON co.order_id = ro.order_id
WHERE ro.cancellation IS NULL
GROUP BY co.order_id
ORDER BY pizza_delivered DESC;

-- 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
SELECT co.customer_id, 
	  COUNT(CASE WHEN (co.exclusions IS NOT NULL OR co.extras IS NOT NULL) THEN pizza_id END) AS changed_orders,
	  COUNT(CASE WHEN co.exclusions IS NULL AND co.extras IS NULL THEN pizza_id END) AS unchanged_orders
FROM customer_orders co
JOIN runner_orders ro
ON co.order_id = ro.order_id
WHERE ro.cancellation IS NULL
GROUP BY co.customer_id;

-- 8.How many pizzas were delivered that had both exclusions and extras?
SELECT customer_id, count(pizza_id) AS pizzas_delivered
FROM customer_orders c
JOIN runner_orders r
ON c.order_id = r.order_id
WHERE cancellation IS NULL
AND exclusions IS NOT NULL AND extras IS NOT NULL
GROUP BY customer_id
ORDER BY pizzas_delivered

--9. What was the total volume of pizzas ordered for each hour of the day?
SELECT COUNT(pizza_id)AS pizza_ordered, DATEPART(hour From order_time) AS order_hour
FROM customer_orders
GROUP BY DATEPART(hour From order_time)
ORDER BY DATEPART(hour From order_time)

--10. What was the volume of orders for each day of the week?
SELECT DATENAME(Weekday, order_time) AS day_of_week, count(order_id) AS count_of_orders
FROM customer_orders
GROUP BY DATENAME(Weekday, order_time)
ORDER BY count_of_orders DESC

