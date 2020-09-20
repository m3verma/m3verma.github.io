---
layout: default
---

# What is Machine Learning?


Arthur Samuel in 1959 coined the term Machine Learning as follows :

> It's a field of study that gives computers the ability to learn without being explicitly programmed

Seems old right?

Tom Mitchell in 1998 gave another definition for Machine Learning as follows :

> A computer program is said to learn from Experience 'E' with respect to some task 'T' and some performance measure 'P', if its performance on 'T' as measured by 'P' improves with experience 'E'

Seems confusing??? Let's give you an example :

```
Suppose you have to create a ML model to identify if an email is a spam or not
So in this case
T = Classifiying emails as spam or not
E = ML Model watching you labeling an email as spam
P = Number of times it correctly identified if an email is spam or not
```

Some examples of where machine learning is useful/or being used :

> DataBase Mining - Large data sets are being used by websites (Web click data - used to check what user is looking for), medical records are used to identify diseases
> Autonomous helicopters - How to fly them
> Natural Language Processing - Identifying meaning of a sentence
> Amazon and Netflix product recommendations

We just barely scratched the surface of machine learning. Now you know what is machine learning let's go in more depth.

## Different types of machine learning : 
### 1. Supervised Learning - 
In supervised learning, we are given a data set and already know what our correct output should look like, having the idea that there is a relationship between the input and the output. In other terms "Right Answers are given to us"
### 2. Unsupervised Learning -
Unsupervised learning allows us to approach problems with little or no idea what our results should look like. We can derive structure from data where we don't necessarily know the effect of the variables. We can derive this structure by clustering the data based on relationships among the variables in the data.
### 3. Reinforcement Learning
### 4. Recommender System

## Supervised Learning is further classified into 2 types - 
### 1. Regression Problems - 
In a regression problem, we are trying to predict results within a continuous output, meaning that we are trying to map input variables to some continuous function. For eg - You want to create a house price prediction ML model. It will try to predict the value of the house based the inputs you provide (Like area of your house). Now this is a contigous function as the output (price) can range from (1 to thousands of Dollars).
### 2. Classification Problem - 
In a classification problem, we are instead trying to predict results in a discrete output. In other words, we are trying to map input variables into discrete categories. For eg - You want to create a model which will identify if a cancer is malignant or benign. So based on your inputs(Like tumor size etc) it will give either of these outputs only.

## Unsupervised Learning is further classified into 2 types - 
### 1. Clustering - 
“Clustering” is the process of grouping similar entities together. The goal of this unsupervised machine learning technique is to find similarities in the data point and group similar data points together. In clustering problems, the algorithms do not transform or change the data, it just groups them according to some characteristics.
### 2. Non Clustering - 
In non-clustering problems, the algorithm is transforming the data/input. A common non-clustering problem would be the Cocktail Party algorithm. Let’s say there are 2 people at a party, and there are two microphones recording from two different positions. Mic 1 is closer to Person 1, and Mic 2 is closer to Person 2. Both mics can pick up the conversation, however, the voice of the closest person is louder. After putting the recording through the Cocktail Party Algorithm, the outputs would have separated the two voices. It allows you to find structure in a chaotic environment, and it is transforming the data. Therefore this would be a non-clustering algorithm.

* * *
