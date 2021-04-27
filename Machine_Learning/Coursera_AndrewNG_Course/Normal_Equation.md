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

# Normal Equation

Usually finding the best model parameters is performed by running some kind of optimization algorithm (e.g. gradient descent) to minimize a cost function. However, it is possible to obtain values (weights) of these parameters by solving an algebraic equation called the normal equation as well. Till now we were using gradient descent to calculate parameters θ<sub>0</sub>,θ<sub>1</sub>. But we can use Normal Equation to solve for θ analytically.

## Intuition

How it normally works? 

Suppose we have a cost function :
> J(θ) = aθ<sup>2</sup> + bθ + c

We will create derivative of it.
> $\Large\frac{d}{dθ_1}$ J(θ) = It will give some value; Let's call it 'Z'

Then we will put Z = 0 and solve for θ. It will give minimum value for θ. But in our regular cost function, it is not that easy.
> J(θ<sub>0</sub>, θ<sub>1</sub>, .. θ<sub>n</sub>) = $\Large\frac{1}{2m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2$<br>
> $\Large\frac{d}{dθ_j}$ J(θ) = ... = 0 (for every j) and solve θ<sub>0</sub>, θ<sub>1</sub>, .. θ<sub>n</sub>

## Example

Suppose your friend is selling his/her house, and you want to create a model which will predict the price of the house so that your friend don't incur a loss. So for you to create a model you will first need to collect details for houses in your friends locality. 

| Size (x<sub>1</sub>) | No of bedrooms (x<sub>2</sub>) | No of floors (x<sub>3</sub>) | Age of home (x<sub>4</sub>) | Price (y) |
|:---------|:----------|
| 2104     | 5     | 1      | 45     | 460       |
| 1416     | 3     | 2      | 40     | 232       |
| 1534     | 3     | 2      | 30     | 315       |
| 852      | 2     | 1      | 36     | 178       |

We can solve Normal equations using matrices. Let us define X, Y matrix as :
> X<br>
> 1 2104 5 1 45<br>
> 1 1416 3 2 40<br>
> 1 1534 3 2 30<br>
> 1 852  2 1 36<br>
> Y<br>
> 460<br>
> 232<br>
> 315<br>
> 178<br>

Actually solving these equations and derivation, we wont go into that detail. But actual formula will be :
> θ = (X<sup>T</sup>X)<sup>-1</sup>X<sup>T</sup>Y<br>

So after putting values of X and Y in the above equations we will get perfect values / minimum values of θ.

### Thing to consider

As we have seen in gradient descent that feature scaling have a lot of importance in speeding up the algorithm. But since in Normal equation their is no iteration, hence feature scaling is not at all required in normal equations.

## Difference

| Gradient Descent | Normal Equation |
|:---------|:----------|
| 1. Learning rate required     | 1. Learning rate not required      |
| 2. Iterations is required     | 2. No iterations required      |
| 3. Works well with large number of features     | 3. Slow for large number of features      |
| 4. Complexity - O(kn<sup>2</sup>)     | 4. Complexity - O(n<sup>3</sup>)      |

## Noninvertible

Now in Linear Algebra section we learnt that only a few matrices have inverse. So what if X<sup>T</sup>X doesnt have an inverse?

It is a very rare scenerio in which it can happen but if it did happen, it can happen due to 2 reasons :
1. Redundant feature - It means that your data contains some redundant feature. Like 2 columns both of size but one in feet and another in meters.
2. Too many feature - When your training data is less then the number of features.

So if we solve the above 2 reason we can make the matrix invertible. Deleting redundant feature and reducing the no of features.
