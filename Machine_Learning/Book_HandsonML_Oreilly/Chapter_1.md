---
layout: default
---

# The Machine Learning Landscape

### What is Machine Learning?

Machine Learning is the science of programming computers so they can learn from data. Some general definitions :

> It is the field of study that gives computers the ability to learn without being explicitly programmed. - Arthur Samuel, 1959

> A computer program is said to learn from experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E. - Tom Mitchell, 1997

Spam filter program is an example of Machine Learning program. It uses example emails to learn if its spam or not. The examples that the system uses to learn are called **training set**. Each training example is called **training instance**. In this case the task T is to flag spam for new emails, the experience E is the training data and performance measure P is the accuracy by which it is assigning emails as spam.

### Why use Machine Learning?

First lets see how we would write the spam filter program with traditional programming :

1. First we need to find what typically looks like spam. Like - words, sender's name etc.
2. Then we would have to write a detection algorithm for each pattern that we noticed, and our program would flag emails as spam if pattern detected.
3. We need to repeat above steps until it is good enough to launch.

![ML_Approach](https://m3verma.github.io/Machine_Learning/Book_HandsonML_Oreilly/Images/Chapter_1/ML_Approach.png)

Now if we would have used Machine Learning techniques our program would have automatically learned the words to identify if a email is spam or not. These programs are much shorter, easier to maintain and most likely more accurate. Moreover they get updated automatically without manual interventaion. Another area where machine learning shines is for the problems that either are too complex for traditional approach or have no known algorithm. For eg. Speech recognition. Finally, Machine Learning can help humans learn. For eg. once ML learns about which email is spam or not, that list can be provided to humans.

Applying ML techniques to dig into large amounts of data can help discover patterns that were not immediately apparent. This is called data mining. To summarize ML is great for :

1. Problems for which existing solution require a lot of fine tuning.
2. Complex problems for which traditional approach yields no solution.
3. Fluctuating environments.
4. Getting insights about complex problems.

### Examples of Machine Learning

1. Analyzing images of products on a production line to automatically classify them.
2. Detecting tumors in brain scans
3. Automatically classifying news articles.
4. Automatically flagging offensive comments on discussion forums.
5. Summarizing long documents automatically.
6. Detecting credit card fraud.
7. Building an intelligent bot for a game.

## Types of Machine Learning systems

Generally systems are classified into 3 broad categories :

1. If trained under human supervision or not.
2. If they can learn on the fly.
3. Simple compare or detecting patterns.

### Supervised Learning

In this the training set we feed to algorithm also includes the desired solutions called labels. A typical supervised learning task is classification (Spam filter). Another typical task is predicting a target numerical value called regression (Price of car). Note that some of the regression problems can be used for classification as well and vice versa. Some important supervised algorithms :

1. k-Nearest Neighbors
2. Linear Regression
3. Logistic Regression
4. Support Vector Machine
5. Decision trees and Random Forests
6. Neural networks

### Unsupervised Learning

In this the training data is unlabeled. The system tries to learn without a teacher.

![Unsupervised_Learning](https://m3verma.github.io/Machine_Learning/Book_HandsonML_Oreilly/Images/Chapter_1/Unsupervised_1.png)

Some of the most important unsupervised learning algorithms :

1. Clustering
 - K-Means
 - DBSCAN
 - Hierarchical Cluster Analysis
2. Anomaly detection and novelty detection
 - One-class SVM
 - Isolation Forest
3. Visualization and dimensionality reduction
 - Principal Component Analysis
4. Association rule learning
 - Apriori
 
 Eg. - If we have alot of data about our blog's visitor. We may want to run a clustering algorithm to try to detect groups of similar visitors. At no point we tell the system which visitor belongs to which category. The system tries to find it automatically. In Visualization algorithms we feed a lot of complex data and they give output a 2D or 3D representation of our data.

- A related task is dimensionality reduction, in which we simply the data without losing too much information. One way it can be done by merging two or more features. For eg. - Car age and mileage can be merged into wear and tear. This is also called feature extraction.

### Semisupervised Learning

Since labeling data is usually time consuming and costly. Some algorithms can deal with data that is partially labelled. This is called semisupervised learning. For eg. - Google photos. Once you upload pictures and tag one person in one picture it automatically recognizes that same person is shown in other photos as well.

### Reinforcement Learning

The learning system called an agent in this context can observe the environment select and perform actions and get rewards in return. It must then learn by itself what is best strategy called policy. A policy defines what action the agent should choose when it is in a given situation.

![Reinforcement_Learning](https://m3verma.github.io/Machine_Learning/Book_HandsonML_Oreilly/Images/Chapter_1/Reinforcement.png)

### Batch Learning




