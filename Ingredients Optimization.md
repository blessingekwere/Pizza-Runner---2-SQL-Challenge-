```sql
-1. What are the standard ingredients for each pizza?
SELECT Distinct toppings
From (Select prc.pizza_id,
     STRING_AGG(pt.topping_name, ', ') AS toppings
FROM #pizza_recipes_clean AS prc
JOIN pizza_toppings AS pt
ON prc.toppings = pt.topping_id
GROUP BY prc.pizza_id) AS sub;

Alter Table pizza_toppings
Alter Column topping_name varchar(255)

--2 What was the most commonly added extra?
SELECT TOP 1 Count(c.extras) AS extra_count
From customer_orders  c
Where c.extras IS NOT NULL

--3 What was the most common exclusion?
SELECT TOP 1 Count(c.exclusions) AS exclusion_count
From customer_orders  c
Where c.exclusions IS NOT NULL

--4 Generate an order item for each record in the customers_orders table in the format of one of the following:
Meat Lovers
Meat Lovers - Exclude Beef
Meat Lovers - Extra Bacon
Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers
Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients
For example: 

IF OBJECT_ID('tempdb..#extras_separated') IS NOT NULL
    DROP TABLE #extras_separated;

CREATE TABLE #extras_separated (
    row_number INT,
    extra_id INT
);

INSERT INTO #extras_separated (row_number, extra_id)
SELECT
    nco.row_number,
    CAST(TRIM(value) AS INT) AS extra_id
FROM
    customer_orders nco
CROSS APPLY STRING_SPLIT(nco.extras, ',')
WHERE
    value <> '';

IF OBJECT_ID('tempdb..#exclusions_separated') IS NOT NULL
    DROP TABLE #exclusions_separated;

CREATE TABLE #exclusions_separated (
    row_number INT,
    exclusions_id INT
);

INSERT INTO #exclusions_separated (row_number, exclusions_id)
SELECT
    nco.row_number,
    CAST(TRIM(value) AS INT) AS exclusions_id
FROM
    customer_orders nco
CROSS APPLY STRING_SPLIT(nco.exclusions, ',')
WHERE
    value <> '';

WITH extras_CTE AS (
    SELECT
        es.row_number,
        CONCAT('Extra ', STRING_AGG(pt.topping_name, ', ')) AS options
    FROM
        #extras_separated AS es
    JOIN
        pizza_toppings AS pt ON es.extra_id = pt.topping_id
    GROUP BY
        es.row_number
),
exclusions_CTE AS (
    SELECT
        exs.row_number,
        CONCAT('Exclude ', STRING_AGG(pt.topping_name, ', ')) AS options
    FROM
        #exclusions_separated AS exs
    JOIN
        pizza_toppings AS pt ON exs.exclusions_id = pt.topping_id
    GROUP BY
        exs.row_number
),
Union_CTE AS (
    SELECT *
    FROM extras_CTE
    UNION
    SELECT *
    FROM exclusions_CTE
)
SELECT
    nco.row_number,
    nco.order_id,
    nco.customer_id,
    nco.pizza_id,
    nco.order_time,
    CONCAT_WS(' - ', pn.pizza_name, STRING_AGG(u.options, ' - ')) AS pizza_information
FROM
    customer_orders nco
LEFT JOIN
    Union_CTE u ON nco.row_number = u.row_number
JOIN
    pizza_names pn ON nco.pizza_id = pn.pizza_id
GROUP BY
    nco.row_number,
    nco.order_id,
    nco.customer_id,
    nco.pizza_id,
    nco.order_time,
    pn.pizza_name
ORDER BY
    nco.order_id,
    nco.row_number;


--5 Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"
WITH Ingredient AS (
	SELECT 
		c.record_id, 
		c.order_id, 
		c.customer_id, 
		c.pizza_id, 
		pn.pizza_name,
		CASE WHEN tb.toppings_id IN(
		SELECT e.extra_id
		FROM extra_break AS e
		WHERE e.record_id = c.record_id) THEN '2x ' || pt.topping_name
		  ELSE pt.topping_name
			 END AS topping
	FROM customer_orders AS c
	INNER JOIN toppings_break AS tb
	USING(pizza_id)
	INNER JOIN pizza_names AS pn
	USING(pizza_id)
	INNER JOIN pizza_toppings AS pt
	ON tb.toppings_id = pt.topping_id
	WHERE 
	  NOT EXISTS 
	  (SELECT 1 
		FROM exclusion_break eb 
		 WHERE c.record_id = eb.record_id 
		 AND pt.topping_id = eb.exclusions_id))
		 
SELECT record_id, order_id, customer_id, pizza_id,
CONCAT(pizza_name, ': ', STRING_AGG(topping, ' , ')) AS ingredient_list
FROM ingredient
GROUP BY 
	order_id,
	customer_id, 
	pizza_id, 
	pizza_name, 
	record_id
ORDER BY order_id;
	
	
-- 6. What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?

WITH used_times AS(
SELECT
	pt.topping_name,
	CASE 
		WHEN tb.toppings_id IN(
			SELECT extra_id
			FROM extra_break AS e
			WHERE e.record_id = c.record_id) 
		THEN 2
		WHEN tb.toppings_id IN(
			SELECT exclusions_id
			FROM exclusion_break AS eb
			WHERE eb.record_id = c.record_id)
		THEN 0
		ELSE 1 END AS times_used
FROM cust_order AS c
INNER JOIN toppings_break AS tb
USING(pizza_id)
INNER JOIN pizza_names AS pn
USING(pizza_id)
INNER JOIN pizza_toppings AS pt
ON pt.topping_id = tb.toppings_id 
)
SELECT topping_name, SUM(times_used) AS ingredient_qty
FROM used_times
GROUP BY topping_name
ORDER BY ingredient_qty


--6 What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?
-- ----------------------------------------------------------------------------------------------------------------------------------
-- Create a temporary table 'new_recipe' that shows pizza_id, topping_id and topping_name from pizza_recipe and pizza_toppings table
-- ----------------------------------------------------------------------------------------------------------------------------------

DROP #new_recipe;
CREATE #new_recipe (
SELECT sub.*, topping_name
FROM (
SELECT 
    pr.pizza_id,
    CAST(TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(pr.toppings, ',', n.n), ',', -1)) AS UNSIGNED) AS topping_id
FROM 
    pizza_recipes pr
CROSS JOIN 
    (
        SELECT 1 AS n UNION ALL
        SELECT 2 UNION ALL
        SELECT 3 UNION ALL
        SELECT 4 UNION ALL
        SELECT 5 UNION ALL
        SELECT 6 
    ) n
WHERE 
    n.n <= LENGTH(pr.toppings) - LENGTH(REPLACE(pr.toppings, ',', '')) + 1
ORDER BY pizza_id) sub
JOIN pizza_toppings AS pt
ON sub.topping_id = pt.topping_id
);


WITH total_ingredients AS (
  SELECT 
    nco.row_number,
    nr.topping_name,
    CASE
      -- if extra ingredient, add 2
      WHEN nr.topping_id IN (
          SELECT extra_id 
          FROM extras_seperated es
          WHERE es.row_number = nco.row_number) 
      THEN 2
      -- if excluded ingredient, add 0
      WHEN nr.topping_id IN (
          SELECT exclusions_id 
          FROM exclusions_seperated exs 
          WHERE nco.row_number = exs.row_number)
      THEN 0
      -- no extras, no exclusions, add 1
      ELSE 1
    END AS quantity
  FROM customer_orders nco
  JOIN new_recipe nr
    ON nr.pizza_id = nco.pizza_id
  JOIN pizza_runner.pizza_names pn
    ON pn.pizza_id = nco.pizza_id
)
SELECT 
  topping_name,
  SUM(quantity) AS quantity_used 
FROM total_ingredients
GROUP BY topping_name
ORDER BY quantity_used DESC;
