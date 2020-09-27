---
layout: default
---


 <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 

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

So, if we try to fit a straight line across these points, it's called Linear Regression.

Since we have only 1 X variable it is called linear regression with 1 variable.

![House_Prediction_Plot](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Linear_Regression_1/house_price_problem_1.png)

Now this data set (data which we have collected) is also called **Training set**.

Some of the notation which will be used through out -
```
m = Total number of data sets
x = Input variable or Feature
y = Output variable or Target
(x,y) = Any training set example
```

* * *

## How ML model is created?

The training set (data) is fed to a learning algorithm which creates a **h** function also called hypothesis function. This function take X as input and gives Y as output.

![flow_chart](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Linear_Regression_1/flow_chart.png)

So for our example the machine learning algorithm will create an h function (hypothesis function) which will take size as input and will provide prediction of cost of the house as output. 

It will be denoted as -
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x

In mathematical notation -
> h : x -> y

Now, our task is to find the value of θ<sub>0</sub> and θ<sub>1</sub>

* * *

## Cost function

Let's assume some values of θ<sub>0</sub> and θ<sub>1</sub>

Below we tried to guess the values of θ<sub>0</sub> and θ<sub>1</sub> and plotted them onto the X and Y axis

![Values of Thetha](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Linear_Regression_1/Thetha_Values.png)

But still we need to determine what will be the perfect value for θ<sub>0</sub> and θ<sub>1</sub>

The perfect value of θ<sub>0</sub> and θ<sub>1</sub> is that which will make h(x) as close as possible to y.

In simpler terms :
> The difference betwwen the derived **'y'** and the actual **'y'** should be minimum.

In mathematical terms :
> Minimize -> $\Large\frac{1}{2m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2 $

> where $h_\theta(x^{(i)}) = \theta_0 + \theta_1x^{(i)}$

The above equation is called **Cost Function** which is denoted by - J(θ<sub>0</sub>, θ<sub>1</sub>)

This mathematical notation is also called Squared Error Function or Mean Squared Error which is commonly used in regression problems.
