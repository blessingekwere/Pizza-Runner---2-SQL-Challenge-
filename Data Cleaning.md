``` sql
_pizza_names column
* Renaming the pizza name
UPDATE pizza_names
SET pizza_name = 'Meat Lovers'
WHERE CAST(pizza_name AS varchar(max)) = 'Meatlovers'

* Changing the data type for pizza_name in the Pizza_names column from TEXT to varchar
ALTER TABLE pizza_names
ALTER COLUMN pizza_name varchar(255)

_customer_orders table
* Replacing the empty records with "Null"
UPDATE customer_orders
SET exclusions = 'Null'
Where exclusions = 'null'

UPDATE customer_orders table
SET extras = 'Null'
Where extras= ' ' OR extras = 'NULL' OR extras IS NULL

_runner_orders table
* Cleaning cancellation column
UPDATE runner_orders
SET cancellation = 'Null'
WHERE cancellation IS NULL

* Cleaning duration column
UPDATE runner_orders
SET duration = REPLACE(duration, 'minutes', '')

UPDATE runner_orders
SET duration = REPLACE(duration, 'min', '')

* Cleaning the distance column
UPDATE runner_orders
SET distance = REPLACE(distance, 'km', '')

UPDATE runner_orders
SET duration = 'Null'
WHERE duration = 'null'

UPDATE runner_orders
SET distance = 'Null'
WHERE  distance = 'null'

 ```
