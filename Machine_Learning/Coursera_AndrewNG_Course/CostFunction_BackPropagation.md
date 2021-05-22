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

# Cost Function

In previous section we talked about using neural network to create a Logical AND, OR, and XNOR gate. In those examples we assumed the values of θ. But how do we actually calculate them? To do that first we need to define cost function of neural network. To do that lets see a neural network :

![MultiClass](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/MultiClass.png)

Let's define some terminologies :
> L = Total number of layers in network<br>
> S<sub>l</sub> = Number of units (not counting bias unit) in layer l<br>
> K = Number of output units/classes 

So in our above example L = 4 and S<sub>4</sub> = 4. Now for binary classification :
> S<sub>L</sub> = 1<br>
> k = 1 

For multiclass classification
> S<sub>L</sub> = k<br>
> k >= 3 

Recall that in neural networks, we may have many output nodes. We denote $h_θ(x)$<sub>k</sub> as being a hypothesis that results in the $k^{th}$ output. Our cost function for neural networks is going to be a generalization of the one we used for logistic regression. Recall that the cost function for regularized logistic regression was :

> J(θ) = - $\Large\frac{1}{m}$ $\sum_{i=1}^m ( ylogh_\theta(x) + (1 - y)log(1 - h_\theta(x)))$ + $\frac{\lambda}{2m}\sum_{j=1}^{m}\theta_j^2$

For neural networks, it is going to be slightly more complicated :

> J(θ) = - $\Large\frac{1}{m}$ $\sum_{i=1}^m\sum_{k=1}^K ( y_k^{(i)}logh_\theta(x^{(i)})$<sub>k</sub> + $(1 - y^{(i)}$<sub>k</sub>$)log(1 - h_\theta(x^{(i)})$<sub>k</sub>$))$ + $\frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{S_l}\sum_{j=1}^{S_{l+1}}(\theta_{ji}^{(l)})^2$

We have added a few nested summations to account for our multiple output nodes. In the first part of the equation, before the square brackets, we have an additional nested summation that loops through the number of output nodes. In the regularization part, after the square brackets, we must account for multiple theta matrices. The number of columns in our current theta matrix is equal to the number of nodes in our current layer (including the bias unit). The number of rows in our current theta matrix is equal to the number of nodes in the next layer (excluding the bias unit). As before with logistic regression, we square every term.

Note :
1. The double sum simply adds up the logistic regression costs calculated for each cell in the output layer
2. The triple sum simply adds up the squares of all the individual Θs in the entire network.
3. The i in the triple sum does not refer to training example i

# Backpropagation

So, in previous topic we learnt the cost function of neural network. No we need to minimize it. "Backpropagation" is neural-network terminology for minimizing our cost function, just like what we were doing with gradient descent in logistic and linear regression. Our goal is to compute :
> min J(θ)

That is, we want to minimize our cost function J using an optimal set of parameters in theta. In this section we'll look at the equations we use to compute the partial derivative of J(Θ) :
> $\Large\frac{d}{dθ_{ij}^{(l)}}$ J(Θ)

To this let's consider the scenerio when we have just 1 training example (x,y), and we have a neural network like this :

![MultiClass](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Neural_Network/MultiClass.png)

So first we calculate forward propagation with some random value of θ :
> a<sup>(1)</sup> = x<br>
> z<sup>(2)</sup> = θ<sup>(1)</sup>a<sup>(1)</sup><br>
> a<sup>(2)</sup> = g(z<sup>(2)</sup>) <br>
> z<sup>(3)</sup> = θ<sup>(2)</sup>a<sup>(2)</sup><br>
> a<sup>(3)</sup> = g(z<sup>(3)</sup>) <br>
> z<sup>(4)</sup> = θ<sup>(3)</sup>a<sup>(3)</sup><br>
> a<sup>(4)</sup> = h<sub>θ</sub>(x) = g(z<sup>(4)</sup>) <br>

Now we calculate 
> $\delta_j^{(l)}$ = "Error" of node j in layer l

We start from last layer as we know what output it will be :
> $\delta_j^{(4)}$ = $a_j^{(4)} - y_j$ Difference between hypothesis output and actual output<br>
> $\delta_j^{(4)}$ = $(h_θ^{(x)})$<sub>j</sub> - $y_j$

Now we compute delta for earlier terms
> $\delta_j^{(3)}$ = $(θ^{(3)})^T\delta^{4} .* a^{(3)} .* (1 - a^{(3)})$<br>
> $\delta_j^{(2)}$ = $(θ^{(2)})^T\delta^{3} .* a^{(2)} .* (1 - a^{(2)})$<br>

Back propogation term is coined because we start at last layer and work our way backward to reduce the error.

## Algorithm

- Training set {$(x^{(1)}, y^{(1)}), ... , (x^{(m)}, y^{(m)})$}<br>
- Set $\Delta_{ij}^{(l)} = 0 for all i,j,l$
- for i=1 to m :
 - sd
