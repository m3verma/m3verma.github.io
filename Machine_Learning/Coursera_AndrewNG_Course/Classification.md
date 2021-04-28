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

# Classification problems

In machine learning, classification refers to a predictive modeling problem where a class label is predicted for a given example of input data. Examples of classification problems include:

1. Given an example, classify if it is spam or not.
2. Given a handwritten character, classify it as one of the known characters.
3. Given recent user behavior, classify as churn or not.

From a modeling perspective, classification requires a training dataset with many examples of inputs and outputs from which to learn.A model will use the training dataset and will calculate how to best map examples of input data to specific class labels. As such, the training dataset must be sufficiently representative of the problem and have many examples of each class label. Class labels are often string values, e.g. “spam,” “not spam,” and must be mapped to numeric values before being provided to an algorithm for modeling.

## Tumor Prediction Problem

Suppose we want to create a model which will predict if a tumor is malignant or benign. So for you to create a model you will first need to collect details for the tumor. As of now we will assume only size of the tumor is enough (but in real world its not) to predict if its malignant or benign.

![Tumor Problem](https://m3verma.github.io/Machine_Learning/Coursera_AndrewNG_Course/Images/Classification/Tumor_Problem.png)

So in the above graph we can draw X-axis as tumor size and Y-axis as 2 outputs :
> 0 : Tumor is benign <br>
> 1 : Tumor is malignant

Now we can try to fit linear regression algorithm in this classification problem. So we will have :
> h(x) = θ<sup>T</sup>X

Our prediction should be of only 2 outputs i.e either 1 or 0 but linear regression gives output as a Real number. So we can try to put threshold classifier at (let's say) 0.5 :
> if h(x) >= 0.5, predict "y=1"
> if h(x) < 0.5, predict "y=0"

But linear regression doesn't work properly in the cases of classification as when we work on a real word problem, at that moment no straight line can be fit in our data. So if it is working in your case then you might be lucky :) . One more issue is their with linear regression that h(x) value can be less than 0 and greater than 1 even but we want only values either 0 or 1. So it becomes tough to create a demarcation of what to take as 0 and 1.
