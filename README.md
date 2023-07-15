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

* [Pricing and Ratings ](https://github.com/blessingekwere/Pizza-Runner---2-SQL-Challenge-/blob/main/Pricing%20and%20Ratings.md)

### Insights
The following insights were generated from the analysis

* A total of 14 orders were placed by 10 distinct customers.
  
* Runner 1 had the highest number of successful deliveries, completing 4 orders. This indicates their efficiency and activity compared to the other runners.
  
* MeatLovers pizzas were more popular, with 9 delivered, while Vegetarian pizzas had 3 deliveries.
  
* Only one pizza had both extra toppings and exclusions specified, suggesting customers typically choose one customization option over the other.
  
* Saturdays and Wednesdays had the most orders, while Fridays had the least.
  
* Week one had the highest number of runner sign-ups, with 2 joining the platform.
  
* There is a positive relationship between average delivery time and pizza quantity, meaning larger orders tend to take longer to deliver.
  
* Customer 105 has the highest average distance traveled, indicating their delivery locations are farther from the pizza store compared to other customers.
  
* Runner 2 is the fastest rider.
  
* Runner 1 has the highest delivery rate.
  
* Bacon is the most commonly added extra, while Cheese is the most common exclusion.
  
* Mushrooms and Bacon are the most frequently used ingredients.
  
* The restaurant generated a total revenue of $138.
  
* Pizza Runner has a total of $94.44 after paying the riders.

### Recommendation

Based on the insights gained from the analysis, the following recommendations can be made:

* Runner Performance: Recognize and reward Runner 1 for their outstanding performance in completing the highest number of successful deliveries. Consider providing incentives to motivate and retain high-performing runners.

* Pizza Variety: Given the higher demand for MeatLovers pizzas compared to Vegetarian options, consider expanding the MeatLovers pizza variations and offerings to cater to customer preferences. This can help increase customer satisfaction and attract more orders.

* Customization Options: Since most customers choose either extra toppings or exclusions rather than combining both in a single order, streamline the customization process by offering clear options for toppings and exclusions. This will simplify the ordering experience and reduce potential confusion.

* Delivery Optimization: Analyze the order patterns to identify peak days and allocate more resources, such as runners, on Saturdays and Wednesdays when the majority of orders are placed. This will help ensure timely deliveries and enhance customer satisfaction.

* Runner Recruitment: As Week one had the highest number of runner sign-ups, consider implementing targeted recruitment strategies during this period to attract more runners to the platform. Increasing the number of runners will improve delivery capacity and reduce delivery times.

* Delivery Time and Order Size: Monitor the relationship between average delivery time and pizza quantity. Consider optimizing delivery logistics and processes to minimize the impact of larger orders on delivery time. Efficient routing and scheduling can help improve overall delivery speed.

* Customer Satisfaction: Engage with Customer 105 to understand their specific needs and ensure their satisfaction. Provide additional support or incentives to maintain their loyalty and enhance their overall experience.

* Runner Training: Leverage the expertise and efficiency of Runner 2 to train other runners and share best practices. This will help improve the overall speed and quality of deliveries across the runner team.

* Ingredient Management: Based on the popularity of Bacon as an added extra and Cheese as a common exclusion, ensure sufficient stock of Bacon and offer alternative cheese options to cater to diverse customer preferences. This will prevent stock shortages and provide customers with the desired customization options.

* Financial Sustainability: Maintain a balanced approach to revenue and expenses. Continuously review and optimize pricing, delivery fees, and operational costs to ensure Pizza Runner generates sustainable profits while adequately compensating runners for their services.

By implementing these recommendations, Pizza Runner can enhance customer satisfaction, improve operational efficiency, and strengthen its position in the market, leading to increased profitability and long-term success.

Thank you for going this project.

Your thoughts, comments and recommendations will be highly appreciated🤗

Kindly connect with me on [Twitter](https://twitter.com/Eddie_Gregs?t=dF3996shVxvPJTePTtxDdw&s=09) and [LinkedIn](https://www.linkedin.com/in/blessing-ekwere-857326216)
