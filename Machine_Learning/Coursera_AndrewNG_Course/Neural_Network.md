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

## Neurons

Neural networks was developed in resemblance to neurons in our brain. So to understand how a neural network works, we first need to understand how neurons in our brain work.

![Neurons](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/neuron.png)

> Cell body - Central computational unit<br>
> Dendrites - Input wires<br>
> Axon - Output wires<br>

As you can see in the diagram, basically what a neuron does is that dendrite receives signal from different parts of body or other neurons. The cell body then processes that signal and axon transfers it to other neurons or muscles wherever it is connected. So it is a computational unit which takes a number of inputs does some calculations and transfers the outputs. The neurons can also communicate between each other using electric pulses. It happens when a dendrite of a neuron is connected to axon of another neuron. So second neuron can send message to first neuron.

## Neuron Model

![Neuron_Model](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/neuron_model.png)

This is the simplest representation of a neuron model. Here yellow circle is where the computation happens (cell body). x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub> are Input wires or dendrites through which we get our inputs. h<sub>θ</sub>(x) is the hypothesis which we will get after all the computation is done. It is also the axon of our neuron model. Here we will be using logistic function in computation i.e. :
> h(x) = $\Large\frac{1}{1+e^{-θ^Tx}}$

Due to which this neuron model will be known as Sigmoid (logistic) activation function. Here we might sometimes draw an extra input to simplify calculations called x<sub>0</sub> which will always have value of 1. This extra input is called bias unit. One more important thing to note is that in our previous machine learning algorithms we denoted θ as parameters. In neural networks θ is also known as weights instead of parameters.

## Neural network

The actual simplest neural network looks something like this :

![Neuron_Model](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/Neuron_model_2.png)

As represented in above image :
> Layer 1 - Its called Input layer<br>
> Final layer - Its called Output layer<br>
> Middle layers - Activation units or Hidden layers<br>

Here x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub> are some inputs which goes through activations functions a<sub>1</sub>, a<sub>2</sub>, a<sub>3</sub> and then again on last layer and gives some output. More formally :
> x<sub>i</sub><sup>(j)</sup> = "activation" of unit i in layer j<br>
> θ<sup>(j)</sup> = Matrix of weights controlling function mapping from layer j to layer j+1

Now if you saw that we are using sigmoid function which is represented by g(z), so some computation represented by this particular neural network is :
> $a_1^{(2)} = g(θ_{10}^{(1)}x_0 + θ_{11}^{(1)}x_1 + θ_{12}^{(1)}x_2 + θ_{13}^{(1)}x_3 ) $<br>
> $a_2^{(2)} = g(θ_{20}^{(1)}x_0 + θ_{21}^{(1)}x_1 + θ_{22}^{(1)}x_2 + θ_{23}^{(1)}x_3 ) $<br>
> $a_3^{(2)} = g(θ_{30}^{(1)}x_0 + θ_{31}^{(1)}x_1 + θ_{32}^{(1)}x_2 + θ_{33}^{(1)}x_3 ) $<br>
> Final function = $h_θ(x)$ = $a_1^{(3)} = g(θ_{10}^{(2)}a_0^{(2)} + θ_{11}^{(2)}a_1^{(2)} + θ_{12}^{(2)}a_2^{(2)} + θ_{13}^{(2)}a_3^{(2)} ) $

This might seems to be a lot but actually its pretty straight forward. We created this function similarly to logistic regression hypothesis function. Here :
> a<sub>1</sub> = First hidden unit function<br>
> a<sub>2</sub> = Second hidden unit function<br>
> a<sub>3</sub> = Third hidden unit function<br>
> h = Final function

Here Each layer of neural network get's its own set of θ. The order of θ is dependent on the inputs and activation function. So in this scenerio :
> Input units = 3
> Output units = 3
> Order of θ<sup>(1)</sup> = 3 X 4 matrix
