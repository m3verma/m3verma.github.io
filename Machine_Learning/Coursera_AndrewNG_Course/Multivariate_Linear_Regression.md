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

# Multivariate Linear Regression

This is quite similar to the simple linear regression model we have discussed previously, but with multiple independent variables contributing to the dependent variable and hence multiple coefficients to determine and complex computation due to the added variables. Till now we were only using 1 input feature. Like in our House price prediction example we were trying to predict price of the house just on the size of house. But in practical this is not the only factor on which price of house depends. Their are various other factors like Number of bedrooms, number of floors, age of house etc.

## Updated House Price Problem

Suppose your friend is selling his/her house, and you want to create a model which will predict the price of the house so that your friend don't incur a loss. So for you to create a model you will first need to collect details for houses in your friends locality. 

| Size (x<sub>1</sub>) | No of bedrooms (x<sub>2</sub>) | No of floors (x<sub>3</sub>) | Age of home (x<sub>4</sub>) | Price (y) |
|:---------|:----------|
| 2104     | 5     | 1      | 45     | 460       |
| 1416     | 3     | 2      | 40     | 232       |
| 1534     | 3     | 2      | 30     | 315       |
| 852      | 2     | 1      | 36     | 178       |
| ...      | ...       | ...       | ...       | ...       |

Some of the notation which will be used through out -
> m = Total number of data sets
> n = Number of features
> x<sup>(i)</sup> = Input feature of i<sup>th</sup> example
> x<sup>(i)</sup><sub>j</sub> = Value of feature j in i<sup>th</sup> training example
> y = Output variable or Target

Some example :
> x<sup>2</sup> = 1416, 3, 2, 40<br>
> x<sup>2</sup><sub>3</sub> = 2

* * *

## Hypothesis Function

Previously with just 1 feature our hypothesis function looked like :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x

But now with introduction of multiple features, our function will look like :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + θ<sub>3</sub>x<sub>3</sub> + θ<sub>4</sub>x<sub>4</sub>

An example what this new hypothesis function look like if we substitute θ :
> h(x) = 80 + 0.1x<sub>1</sub> + 0.01x<sub>2</sub> + 3x<sub>3</sub> - 2x<sub>4</sub>

**NOTE** - Here +ve coefficient denotes that that feature increases the price of house and a -ve coefficient denotes that that feature decreases the price of house.

Now if we try to generalize the hypothesis function for n input feature, it would look something like this :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + .... + θ<sub>n</sub>x<sub>n</sub>

For ease of understanding we can even convert the above equation into a matrix form. How ? 
First for convenience of notation let us define : 
> x<sub>0</sub> = 1

Now our generalized hypothesis equation can be written as :
> h(x) = θ<sub>0</sub>x<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + .... + θ<sub>n</sub>x<sub>n</sub>

Secondly lets define 2 matrices :
> x<br>
> x<sub>0</sub><br>
> x<sub>1</sub><br>
> x<sub>2</sub><br>
> :<br>
> :<br>
> x<sub>n</sub><br>
> θ<br>
> θ<sub>0</sub><br>
> θ<sub>1</sub><br>
> :<br>
> :<br>
> θ<sub>n</sub><br>

Now using matrix matrix multiplication method our generalized hypothesis function can be written as :
> h(x) = θ<sup>T</sup>x

The above equation is very simple matrix form of our general hypothesis function.

## Gradient Descent

Gradient descent is a first-order iterative optimization algorithm for finding a local minimum of a differentiable function. The idea is to take repeated steps in the opposite direction of the gradient (or approximate gradient) of the function at the current point, because this is the direction of steepest descent. So this is an algo to find minimum of any function. So here we want :
> min (J(θ<sub>0</sub> , θ<sub>1</sub>, ..., θ<sub>n</sub>))

So our algorithm will become : <br>
Repeat until convergence {
> θ<sub>j</sub> = θ<sub>j</sub> - $\alpha$ $\Large\frac{d}{dθ_j}$ J(θ<sub>0</sub> , θ<sub>1</sub>, ..., θ<sub>n</sub>) <br>
}

Here, θ<sub>0</sub>, θ<sub>1</sub>, ..., θ<sub>n</sub> needs to be updated simultaneously. Now putting our cost function and actual differential calculus we wont be covering here. So let’s directly jump to the outcome after applying the differentiation on our gradient descent algorithm :<br>
Repeat until convergence {
> θ<sub>j</sub> = θ<sub>j</sub> - $\alpha$ $\Large\frac{1}{m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)}).x_j^{(i)}$<br>
}<br>

Here simultaneously update θ<sub>j</sub> for j=0,1,...n, Where x<sub>0</sub><sup>(i)</sup> = 1 as assumed previously.

* * *

## Making Gradient Descent Faster

We can speed up gradient descent by having each of our input values in roughly the same range. This is because θ will descend quickly on small ranges and slowly on large ranges, and so will oscillate inefficiently down to the optimum when the variables are very uneven. The way to prevent this is to modify the ranges of our input variables so that they are all roughly the same. Ideally :

> −1 ≤ $x_{(i)}$ ≤ 1
> or
> −0.5 ≤ $x_{(i)}$ ≤ 0.5

These aren't exact requirements; we are only trying to speed things up. The goal is to get all input variables into roughly one of these ranges, give or take a few. Now let's see what will happen if the scale of features are very different. Like :
> x<sub>1</sub> = Size of house (0-2000 feet) <br>
> x<sub>2</sub> = Number of bedrooms (1-5)

If we try to plot it, it will look something like this :

![Without_Feature_Scaling](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Multiple_Linear_Regression/WOFeatureScaling.png)

Now as you can see, It will be very elliptical curve and hence it will take a very long time to reach global minimum. Now for making features scale to same level their are 2 ways to do it.

### Feature Scaling

Feature scaling involves dividing the input values by the range (i.e. the maximum value minus the minimum value) of the input variable, resulting in a new range of just 1. In this scenerio we can make the scaling more equal by :
> x<sub>1</sub> = $\Large\frac{value}{range}$

By this method our feature will be in range of −1 ≤ $x_{(i)}$ ≤ 1 or atleast close enough. If we try to plot it, it will look something like this :

![With_Feature_Scaling](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Multiple_Linear_Regression/WFeatureScaling.png)

Now as you can see, It will be a circular curve and hence It will take very less time/iterations to reach global minimum.

### Mean Normalization

Mean normalization involves subtracting the average value for an input variable from the values for that input variable resulting in a new average value for the input variable of just zero. In this scenerio we can make scaling more equal by :
> x<sub>1</sub> = $\Large\frac{x_i-μ_i}{s_i}$

Where :
>  x<sub>i</sub> = Feature<br>
>  μ<sub>i</sub> = Mean of feature<br>
>  s<sub>i</sub> = Range of feature<br>

For example, if x<sub>i</sub> represents housing prices with a range of 100 to 2000  and a mean value of 1000, then, x<sub>i</sub> = $\Large\frac{price-1000}{1900}$.

### Gradient Descent : Correctly Working?

Their is a way to check if gradient descent algorithm is working correctly or not for the given cost function. To do that we need to plot a grpah with the number of iteration of gradient descent versus the cost function values. If we plot them it will look something like this :

![Gradient_Descent_working_correctly](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Multiple_Linear_Regression/GD_Work_Correct.png)

Now if your graph is continously decreasing that means your gradient descent algorithm is working correctly and no changes is required. But if your graph is increasing or is a zig-zag graph that means learning rate of your algorithm needs to be changed. Some pointers :
1. If your graph is increasing then learning rate needs to be decreased.
2. If your graph is in a zig-zag format then learning rate needs to be decreased.
3. If learning rate is too small, gradient descent will be slow to convergence.
4. If learning rate is large, gradient descent may not decrease and thus may not converge.

### Creating new features

We can even create new features from given feature. Suppose in our house prediction problem if we were given frontage and depth our hypothesis function would have been looked like :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>xFrontage + θ<sub>2</sub>xDepth

Now we can create a new feature something called - "area". Where :
> area = Frontage X Deapth

So now our hypothesis function will look like :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>xArea

### Polynomial Regression

Sometimes, Linear models doesn't fit well with our data and in that scenerio we can try some polynomial regression. Here our hypothesis function will look like :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>X + θ<sub>2</sub>X<sup>2</sup>
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>X + θ<sub>2</sub>X<sup>2</sup> + θ<sub>3</sub>X<sup>3</sup> 

More details we will see later. But one thing to keep in mind that in polynomial regression, feature scaling is very important.
