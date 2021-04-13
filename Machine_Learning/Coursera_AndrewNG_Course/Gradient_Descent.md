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

# Gradient Descent

Gradient descent is a first-order iterative optimization algorithm for finding a local minimum of a differentiable function. The idea is to take repeated steps in the opposite direction of the gradient (or approximate gradient) of the function at the current point, because this is the direction of steepest descent. So this is an algo to find minimum of any function. In our case J(θ<sub>0</sub> , θ<sub>1</sub>). So here we want :
> min (J(θ<sub>0</sub> , θ<sub>1</sub>))

### Outline

1. Start with some θ<sub>0</sub> and θ<sub>1</sub>. ( Say example - θ<sub>0</sub> = 0 , θ<sub>1</sub> = 0 )
2. Keep changing θ<sub>0</sub> and θ<sub>1</sub> to reduce J(θ<sub>0</sub> , θ<sub>1</sub>) until we hopefully end up at a minimum.

So basically, suppose at first run of the gradient descent we started at x<sub>1</sub>. We will move in a direction in which our function is becoming minimum. Hence we will arrive at y<sub>1</sub>. If we run gradient descent again, we would have started either at x<sub>2</sub> or x<sub>3</sub> and hence we would have reached at either y<sub>2</sub> or y<sub>3</sub>. So, it gives us local minimum depending upon where we are starting in each turn.

![Outline_Gradient_Descent](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Gradient_Descent/3_Outline.png)

## Gradient Descent Algorithm

Repeat until convergence {
> θ<sub>j</sub> = θ<sub>j</sub> - $\alpha$ $\Large\frac{d}{dθ_j}$ J(θ<sub>0</sub> , θ<sub>1</sub>) <br>
}

Here, θ<sub>0</sub> & θ<sub>1</sub> needs to be updated simultaneously. So,
> temp0 = θ<sub>0</sub> - $\alpha$ $\Large\frac{d}{dθ_0}$ J(θ<sub>0</sub> , θ<sub>1</sub>) <br>
> temp1 = θ<sub>1</sub> - $\alpha$ $\Large\frac{d}{dθ_1}$ J(θ<sub>0</sub> , θ<sub>1</sub>) <br>
> θ<sub>0</sub> = temp0 <br>
> θ<sub>1</sub> = temp1

Here, $\alpha$ is called learning rate. It controls how big a step we are taking to move towards convergence. In simplest terms $\Large\frac{d}{dθ_0}$ J(θ<sub>0</sub> , θ<sub>1</sub>) is slope of a tangent on graph point θ<sub>0</sub> , θ<sub>1</sub> .

## Gradient Descent for single parameter

Here we are only considering θ<sub>1</sub>. So gradient descent algorithm will become :
Repeat until convergence {
> θ<sub>1</sub> = θ<sub>1</sub> - $\alpha$ $\Large\frac{d}{dθ_1}$ J(θ<sub>1</sub>) <br>
}

Now lets analyze how the algorithm will run.

### Case 1 :

![Case_1](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Gradient_Descent/Case_1.png)

As seen in the image let's assume we started at the given point. Now we drew a tangent to the point. Now the slope if we calculate it will come a positive integer. So,
> $\Large\frac{d}{dθ_1}$ J(θ<sub>1</sub>) >= 0 <br>
> θ<sub>1</sub> = θ<sub>1</sub> - $\alpha$(+ number)

Now if we repeat the algorithm θ<sub>1</sub> will decrease and we will reach local minimum.

### Case 2 :

![Case_2](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Gradient_Descent/Case_2.png)

As seen in the image let's assume we started at the given point. Now we drew a tangent to the point. Now the slope if we calculate it will come a negative integer. So,
> $\Large\frac{d}{dθ_1}$ J(θ<sub>1</sub>) <= 0 <br>
> θ<sub>1</sub> = θ<sub>1</sub> - $\alpha$(- number)

Now if we repeat the algorithm θ<sub>1</sub> will increase and we will reach local minimum.

## Learning Rate

1. If $\alpha$ is too small, gradient descent will be too slow as it will take small steps to proceed.
2. If $\alpha$ is too large, gradient descent can overshoot the minimum. It may fail to converge or even diverge.

## Gradient Descent at local minimum

![Local_minimum](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Gradient_Descent/Local_minimum.png)

As seen in the image let's assume we started at the given point. Now we drew a tangent to the point. This tangent will be parellel to the x-axis. Hence making the slope be 0. So :
> $\Large\frac{d}{dθ_1}$ J(θ<sub>1</sub>) = 0 <br>
> θ<sub>1</sub> = θ<sub>1</sub> - $\alpha$(0)
 
So it will leave θ<sub>1</sub> unchanged. The algorithm will stop as expected.

## Gradient Descent to our cost function

Now lets apply the gradient descent algorithm to the cost function of linear regression which we studied in last topic. So if we recall :
> h(x) = θ<sub>0</sub> + θ<sub>1</sub>x <br>
> J(θ<sub>0</sub> , θ<sub>1</sub>) = $\Large\frac{1}{2m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})^2 $

Now actual differential calculus we wont be covering here. So let's directly jump to the outcome after applying the differentiation.
> θ<sub>0</sub> = $\Large\frac{1}{m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})$ <br>
> θ<sub>1</sub> = $\Large\frac{1}{m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)}).x^{(i)}$

After all the mathematical calculations, we can apply the below Gradient descent algorithm to our linear regression cost function :<br>
Repeat until convergence {
> θ<sub>0</sub> = θ<sub>0</sub> - $\alpha$ $\Large\frac{1}{m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})$ <br>
> θ<sub>1</sub> = θ<sub>1</sub> - $\alpha$ $\Large\frac{1}{m}$ $\sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)}).x^{(i)}$
}

And now we have successfully understood the gradient descent algorithm as well as applied it to our linear regression cost function.

![Gradient_Descent](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Gradient_Descent/Gradient_Descent.png)

As you can see in the above image, we applied gradient descent algo and we came to a point where our machine learning algorithm will be able to predict the house prices closely as possible.

## Note :

1. The graph of linear regression is a bowl-shaped graph. It is called Convex function. It doesnt have local optima but only 1 global optimum. So what it means that no matter at which point we start our gradient descent algorithm we will always reach the global minimum of the function.
2. This approach of gradient descent is also called batch gradient descent as it uses all the training example for each iteration. So mathematically, it is a little bit slower. 
