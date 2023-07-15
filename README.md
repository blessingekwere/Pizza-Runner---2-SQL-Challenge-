# Pizza-Runner
![](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Pizza%20runner%20Introductory%20Pics.png)

### Project Overview
Did you know that over 115 million kilograms of pizza is consumed daily worldwide? Inspired by this astounding fact, Danny embarked on a mission to revolutionize the pizza delivery industry. Combining his love for 80s retro styling and his belief in the power of pizza, he launched Pizza Runner - an innovative platform that connects hungry customers with a fleet of dedicated runners.

With a dream to secure seed funding and expand his Pizza Empire, Danny realized he needed more than just great pizza. He brilliantly decided to transform the delivery process, transforming Pizza Runner into a dynamic and efficient service. He recruited a team of passionate runners to deliver fresh pizzas from his very own headquarters, and invested in a cutting-edge mobile app developed by skilled freelancers to streamline the ordering experience for customers.

Join Danny on his journey as he dives into the data of Pizza Runner. Through careful analysis, he aims to uncover valuable insights that will drive strategic decisions, optimize operations, and propel Pizza Runner to even greater heights. Stay tuned to witness the data-driven success story of Danny Ma and his Pizza Runner venture! Let's go💪🏃

* *Please note that this is the second challenge on the 8 week SQL challenge by Danny Ma. Click [here](https://8weeksqlchallenge.com/) to access the complete material concerning this challenge*

### ER Diagram
Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.
He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations. All datasets exist within the pizza_runner database schema 
![](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Screenshot%20(87).png)

### About the Dataset
The dataset comprises six tables, each with its own set of columns described below:

**Table 1: runners**

The Runners table captures the registration date for each new runner. It consists of the following columns:
* registration_date: Date of registration for each new runner.
* runner_id: Unique identifier for each runner.
  
**Table 2: customer_Orders**

The Customer_Orders table stores customer pizza orders, with 1 row for each pizza in the order. It includes information about the pizza, exclusions, and extras. The table has the following columns:
* order_id: Unique identifier for each customer order.
* customer_id: Unique identifier for each customer.
* pizza_id: Identifier for the type of pizza ordered.
* exclusions: Ingredient_id values to be removed from the pizza.
* extras: Ingredient_id values to be added to the pizza.
* order_date: Date of the customer order.
  
**Table 3: Runner_Orders**

The Runner_Orders table tracks the assignment of orders to runners. It also indicates whether an order was canceled. The table includes the following columns:
* order_id: Unique identifier for each order.
* runner_id: Unique identifier for each runner assigned to the order.
* pickup_time: Timestamp when the runner arrives at Pizza Runner headquarters to pick up the order.
* distance: Distance the runner had to travel to deliver the order.
* duration: Duration of the delivery.
* cancellation: Indicates if the order was canceled.
  
**Table 4: Pizza_Names**

The Pizza_Names table lists the available pizza options at Pizza Runner. Currently, there are two options: Meat Lovers and Vegetarian. It contains the following columns:
* pizza_id: Identifier for each pizza.
* pizza_name: Name of the pizza (Meat Lovers or Vegetarian).
  
**Table 5: Pizza_Recipes**

The Pizza_Recipes table defines the standard set of toppings for each pizza. It associates the pizza_id with the toppings used in the recipe. The table has the following columns:
* pizza_id: Identifier for each pizza.
* toppings: Standard set of toppings used in the pizza recipe.
  
**Table 6: Pizza_Toppings**

The Pizza_Toppings table lists all the available toppings with their corresponding topping_id values. It includes the following columns:
* topping_id: Unique identifier for each topping.
* topping_name: Name of the topping.

*To access the query for creating the tables with the inputted values, simply click [here](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Creating%20tables%20and%20inputting%20the%20values%20in%20the%20db.md).*

### Skills Demonstrated
Skills demonstrated in the Pizza Runner analysis using SQL Server include:

* SQL Querying
* Joins and Relational Operations
* Data Aggregation
* Data Integrity and Validation
* Problem Solving

### Case Study
* [Data Cleaning](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Data%20Cleaning.md)

* [Pizza Metrics](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Pizza%20Metrics.md)

* [Runner and Customer Experience](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Runner%20and%20Customer%20Experience.md)

* [Ingredients Optimization](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Ingredients%20Optimization.md)
