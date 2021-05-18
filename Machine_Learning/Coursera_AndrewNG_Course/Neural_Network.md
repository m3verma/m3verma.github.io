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

# Neural Network

Till now we learnt about Logistic Regression and Linear Regression. But in practical machine learning problems these are not enough. Let's see why the existing algorithms are not sufficient to solve machine learning problem :

![Non-linear_classification](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/NeuralNetwork_1.png)

As you can see in the above classification example based on 2 features (x<sub>1</sub> and x<sub>2</sub>), if we try to apply logistic regression, we will get an equation like this :
> g(θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + θ<sub>3</sub>x<sub>1</sub>x<sub>2</sub> + ...)

Which is very long already. But in actual real world machine learning problem we will have roughly 100 features (or maybe more)
> x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub>, ... x<sub>100</sub>

Now if we try to create an equation for these many features it will be very big. For example if we just consider features of quadratic terms like (x<sub>1</sub>x<sub>2</sub>, x<sub>2</sub>x<sub>3</sub>, x<sub>3</sub>x<sub>5</sub>, ...), the count will be ~ 5000 features. Quadratic features count roughly grows by n<sup>2</sup>. So to calculate for 5000 features the values of theta will be computationally expensive and might even overfit. Now if we just include cubic features like (x<sub>1</sub>x<sub>2</sub>x<sub>3</sub>, x<sub>2</sub>x<sub>3</sub>x<sub>4</sub>, x<sub>3</sub>x<sub>5</sub>x<sub>7</sub>, ...), the count will be ~ 170000 features. This is dramatically more computationally expensive. So, that's why existing algorithm can't be use in most real world applications.

Another example in Computer vision. Suppose we are making an algorithm which will try to identify if an image is of a car or not. To do that assume we are just talking about 50x50 pixel images. Now these 50x50 pixel images will contain 2500 total pixels. Hence 2500 features (in just greyscale). In this scenerio if we try to take just quadratic features it will create 3 million features which is just impossible to calculate even with super computers. :(

So this is the reason we require a non linear hypothesis to work on large real world problems. Neural Netowrk is an algorithm which tries to mimic brain. It is state of the art technique for many applications.

## The 'one learning' hypothesis

Neural networks are derived from neurons in brain. How it was developed well, there's a theory that human intelligence stems from a single algorithm. The idea arises from experiments suggesting that the portion of your brain dedicated to processing sound from your ears could also handle sight for your eyes. This is possible only while your brain is in the earliest stages of development, but it implies that the brain is -- at its core -- a general-purpose machine that can be tuned to specific tasks. Now since a brain can handle multiple task why not a machine learning algorithm. So using this logic a neural network can handle any type of data as we will see later on.
