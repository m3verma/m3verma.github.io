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

# Logistic Regression

The logistic model (or logit model) is used to model the probability of a certain class or event existing such as pass/fail, win/lose, alive/dead or healthy/sick. Now lets try to find values of 0 for our hypothesis function. But before that let's first layout our initial things. Suppose we have a training set :
> {(x<sup>1</sup>, y<sup>1</sup>), (x<sup>2</sup>, y<sup>2</sup>), (x<sup>3</sup>, y<sup>3</sup>), ..., (x<sup>m</sup>, y<sup>m</sup>)} <br>
> Total m examples <br>
> y belongs in {0,1} <br>
> Our hypothesis function <br>
> h(x) = $\Large\frac{1}{1+e^{-θ^Tx}}$

Now our biggest question is how do we choose values for θ.

## How to choose θ? 

Now if we remember our previous sections of linear regression, we used squared error cost function. Which was :
> J(θ) = $\Large\frac{1}{2m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2 $

Now this method to find the cost worked perfectly for linear regression as it made convex function (A function which has 1 global minimum). But here if we try to apply the cost to our hypothesis function of logistic regression it will create a non-convex function (A function which has many local minimum) and due to which we will not be able to find minimum values of θ properly. So to make it work we have to come up with another solution. To do it, let's define a new cost function :
> { -log(h<sub>θ</sub>(x)) if y = 1 } <br>
> { -log(1 - h<sub>θ</sub>(x)) if y = 0 }

Let's analyze it :
![Logistic_Regression_1](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Logistic_Regression/Logistic_Regression.png)

As you can see in the above plot, this is plot of logistic regression/hypothesis function if we take y=1. It has below advantages :
1. If y=1 & it predicts h(x)=1 then Cost=0, as you can analyze through the plot
2. If y=1 & it predicts h(x)=0 then Cost=∞
So the basic is that if our algo predicts wrongly the algorithm is heavily penalized.

![Logistic_Regression_2](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Logistic_Regression/Logistic_Regression_2.png)

As you can see in the above plot, this is plot of logistic regression/hypothesis function if we take y=0. It has below advantages :
1. If y=0 & it predicts h(x)=0 then Cost=0, as you can analyze through the plot
2. If y=0 & it predicts h(x)=1 then Cost=∞
So the basic is that if our algo predicts wrongly the algorithm is heavily penalized. Our new cost function is working as expected till now. But still this cost function looks little complex. Let's try to simplify it.

## Simplified Cost Function

As we saw in previous section our new cost function looks something like this :
> { -log(h<sub>θ</sub>(x)) if y = 1 } <br>
> { -log(1 - h<sub>θ</sub>(x)) if y = 0 }

But it looks a lot complex, Let's try to combine it within a single line/single equation. So,
> Cost(h<sub>θ</sub>(x), y) = - ylog(h<sub>θ</sub>(x)) - (1 - y)log(1 - h<sub>θ</sub>(x))

But is this new equation correct? Let's check it once :
> If y = 0 then Cost(h<sub>θ</sub>(x), y) = log(h<sub>θ</sub>(x)) br>
> If y = 1 then Cost(h<sub>θ</sub>(x), y) = log(1 - h<sub>θ</sub>(x))

So it works correctly. Now this function is for just 1 training data. We need to generalize it for all m training data. So our new Cost function will become :
> J(θ) = - $\Large\frac{1}{m}$ $\sum_{i=1}^m ( ylogh_\theta(x) + (1 - y)log(1 - h_\theta(x)))$

So now we got our cost function, we just have to find values of θ which will minimize J(θ). And what is better to minimize then our own Gradient Descent.

## Gradient Descent Algorithm

If you remember from past sections Gradient Descent algorithm looks like this :

Repeat until convergence {
> θ<sub>j</sub> = θ<sub>j</sub> - $\alpha$ $\Large\frac{d}{dθ_j}$ J(θ) <br>
}

After substituting the new value of J(θ) and taking derivative (again we wont go into details of actual derivation) it will become :
> θ<sub>j</sub> = θ<sub>j</sub> - $\alpha$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)}).x^{(i)}$

Do you remember the above equation from somewhere? It was exactly the same equation which we used in Linear Regression. So, even if our cost function are different the gradient descent is same for both Logistic Regression and Linear regression. One thing to remember even if gradient descent is same it doesnt mean both algorithm are same. Because in one algo we are predicting the values using sigmoid function and in another we are predicting the values using linear algebra. So both are not same. Since we have used gradient descent here that means all properties of it are followed here.

## Advanced Optimizations

From starting of this course we are just talking about only 1 algorithm to find minimum of a function i.e. Gradient Descent. But their are other algorithms as well :
1. Conjugate gradient
2. BFGS
3. L-BFGS

Now we won't go into detail of how these 3 algorithms work. But they have some advantages and disadvantages over gradient descent :
1. In these you dont need to manually pick $\alpha$
2. These are faster than gradient descent
3. These are more complex to implement and understand than gradient descent
