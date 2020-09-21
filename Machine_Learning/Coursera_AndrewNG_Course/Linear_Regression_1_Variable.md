---
layout: default
---

# What is Linear Regression with one variable?

Linear Regression is a machine learning algorithm based on supervised learning. It performs a regression task. Regression models a target prediction value based on independent variables. It is mostly used for finding out the relationship between variables and forecasting. Different regression models differ based on – the kind of relationship between dependent and independent variables, they are considering and the number of independent variables being used.

Linear regression performs the task to predict a dependent variable value (y) based on a given independent variable (x). So, this regression technique finds out a linear relationship between x (input) and y(output). Hence, the name is Linear Regression.

Sounds tough? Lets analyze with an example.

## House Price Problem

Suppose your friend is selling his/her house, and you want to create a model which will predict the price of the house so that your friend don't incur a loss. So for you to create a model you will first need to collect details for houses in your friends locality. As of now we will assume only size of the house is enough (but in real world its not) to predict the price.

| Size (x) | Price (y) |
|:---------|:----------|
| 2104     | 460       |
| 1416     | 232       |
| 1534     | 460       |
| ...      | ...       |
| ...      | ...       |

You will most likely collect data in the above format. But does that give you any usefull info at the moment?
No. Let's look at this data in a different way.
Let's plot the above data into a graph with size being the X-Axis and price being in Y-Axis.

![House_Prediction_Plot](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Linear_Regression_1/house_price_problem.png)

Now are you able to see any pattern in the data?
