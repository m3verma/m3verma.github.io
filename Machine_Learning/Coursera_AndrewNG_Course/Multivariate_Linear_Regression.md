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
