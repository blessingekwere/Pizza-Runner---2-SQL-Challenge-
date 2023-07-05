``` sql
**Data Cleaning**

--  CUSTOMER_ORDERS TABLE
-- Handling null values and empty strings.
UPDATE customer_orders
SET
  exclusions = NULLIF(NULLIF(exclusions, ''), 'null'),
  extras = NULLIF(NULLIF(extras, ''), 'null');

 -- Handlling null values and empty strings
UPDATE runner_orders
SET
  cancellation = NULLIF(NULLIF(cancellation, ''), 'null'),
  distance = NULLIF(NULLIF(distance, ''), 'null'),
  duration = NULLIF(NULLIF(duration, ''), 'null');

  --Replacing the km, minutes and mins in duration and distance columns 

UPDATE runner_orders
SET distance = REPLACE(distance, 'km', '')

UPDATE runner_orders
Set duration =  REPLACE (duration, 'minute', '')

--Renaming the columns
EXEC sp_rename 'runner_orders.distance', 'distance_km', 'COLUMN';

EXEC sp_rename 'runner_orders.duration', 'duration_min', 'COLUMN'

-- Inputting zero for the null values in duration and distance columns
Update runner_orders
SET duration_minutes = 0
Where duration_minutes IS NULL

Update runner_orders
SET distance_km = 0
Where distance_km IS NULL

IF OBJECT_ID('tempdb..#pizza_recipes_clean') IS NOT NULL
    DROP TABLE #pizza_recipes_clean;

CREATE TABLE #pizza_recipes_clean (
    pizza_id INT,
    toppings VARCHAR(255)
)

INSERT INTO #pizza_recipes_clean (pizza_id, toppings)
SELECT pr.pizza_id,
       REPLACE(SUBSTRING(SUBSTRING(CONVERT(varchar(max), pr.toppings), numbers.n + 1, DATALENGTH(pr.toppings)), 1, 
              CHARINDEX(',', SUBSTRING(CONVERT(varchar(max), pr.toppings), numbers.n + 1, DATALENGTH(pr.toppings))) - 1), ' ', '') AS toppings
FROM (
    SELECT 0 AS n UNION ALL
    SELECT 1 UNION ALL
    SELECT 2 UNION ALL
    SELECT 3 UNION ALL
    SELECT 4 UNION ALL
    SELECT 5 UNION ALL
    SELECT 6 UNION ALL
    SELECT 7
) AS numbers
INNER JOIN pizza_recipes pr ON DATALENGTH(pr.toppings) - DATALENGTH(REPLACE(CONVERT(varchar(max), pr.toppings), ',', '')) >= numbers.n
ORDER BY pr.pizza_id, n;

---- Alter pickup_time column for runner_orders table
UPDATE runner_orders
SET pickup_time = CONVERT(datetime, pickup_time, 120)
Where pickup_time <> 'null'

Update runner_orders
Set pickup_time = NULL
Where pickup_time = 'null'

--Changing data type for duration column in runners table
UPDATE runner_orders
SET distance_km = CAST(distance_km AS INT)
WHERE distance_km IS NOT NULL

ALTER TABLE runner_orders
ALTER COLUMN distance_km FLOAT

 ```
