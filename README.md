# Pizza-Runner---2-SQL-Challenge-
## Project Overview
Did you know that over 115 million kilograms of pizza is consumed daily worldwide? Inspired by this astounding fact, Danny embarked on a mission to revolutionize the pizza delivery industry. Combining his love for 80s retro styling and his belief in the power of pizza, he launched Pizza Runner - an innovative platform that connects hungry customers with a fleet of dedicated runners.

With a dream to secure seed funding and expand his Pizza Empire, Danny realized he needed more than just great pizza. He brilliantly decided to transform the delivery process, transforming Pizza Runner into a dynamic and efficient service. He recruited a team of passionate runners to deliver fresh pizzas from his very own headquarters, and invested in a cutting-edge mobile app developed by skilled freelancers to streamline the ordering experience for customers.

Join Danny on his journey as he dives into the data of Pizza Runner. Through careful analysis, he aims to uncover valuable insights that will drive strategic decisions, optimize operations, and propel Pizza Runner to even greater heights. Stay tuned to witness the data-driven success story of Danny Ma and his Pizza Runner venture! Let's go💪🏃


* [Data Cleaning](
## DATA CLEANING

## Cleaning pizza_names table

* Cleaning pizza_names
UPDATE pizza_names
SET pizza_name = 'Meat Lovers'
WHERE CAST(pizza_name AS varchar(max)) = 'Meatlovers';

* Cleaning customer_orders table
UPDATE customer_orders
SET exclusions = 'Null'
Where exclusions = 'null'

UPDATE customer_orders
SET extras = 'Null'
Where extras= ' ' OR extras = 'NULL' OR extras IS NULL

### Cleaning runner_orders table
  
*  Cleaning cancellation column
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

* Changing the data type for pizza_name in the Pizza_names column from TEXT to Varchar
ALTER TABLE pizza_names
ALTER COLUMN pizza_name varchar(255))
