---
layout: default
---

# Machine Learning 101

Machines are good at certain things like computers can perform certain task at a really fast pace. We can program computers to do those tasks. For example if we want to go to someone's house we have to option - 

1. Take a map and a ruler and map out all the routes from my current location to destination and find the shortest path to take.
2. You can ask computer to tell me how to get to my destination, through programming.

Instead of working 10 mins myself to calculate all possible routes, I can simply press a button and computer will tell me the shortest route. Companies saved a lot of money by using computers as they dont have to hire a ton of workers and computers can do their task instead. But these machines are only good on the task which we can program by using if else programs. For example if we go ahead and program the above code it will look something like this :

```python
if : route 1 < route 2
if : route 1 < route 3
if : route 1 < route 4
if : route 1 < route 5
then : pick route 1
```

Computers are very good at performing task which have set defined rules. Now let's consider another example. We want our machine to detect a person emotion by reading the review he/she posted for a product on Amazon. Now using an if else block we cant describe to a computer what angry means. The harder the things become to describe the harder it is for us to tell machines what to do. So we hire humans to do this tough part and let computers handle the easy part. 
The things which computers were not be able to do before and only humans could do can now be done by machines with machine learning. The goal of machine learning is to make machine act more and more like humans.

![Veinn Diagram](https://m3verma.github.io/Machine_Learning/DanielBourke_Course_CompMLDS/Images/MachineLearning101/Veinn_Diagram.PNG)

It all starts with AI or artificial intelligence which means human intelligence demonstrated by machines. We also have Narrow AI which means where machines are just as good or even better then humans at some tasks. Each AI is only good at one task. Narrow AI is good at just one task, they dont have multiple abilities like humans. Machine Learning is a subset of AI. ML is an approach to reach AI through systems that can find patterns in a set of data.

Stanford ML Definition - Science of getting the computers to act without being explicitly programmed that is getting machines to do things without us specifically saying if this than that.

Deep Learning is just one of the techniques for implementing machine learning.

The field of data science simply means analyzing data looking at data and then doing something with that data. Data Science and Machine Learning are overlapping things. You cant do one without the other.

## Teachable Machine

There is an online project developed by Google - [Teachable Machine](https://teachablemachine.withgoogle.com/)

On this website you can play with some interesting machine learning projects. A Basic one is that you provide machine some images pre-classified in respective groups. The website will analyze them and train a ML model based on the input. After the model is created when ever you will provide a new image the model will try to classify the image into a group.

And that's how exactly a ML machine works. You provide some inputs to the machine. Then you tell it what type of data it is. The machine will learn automatically based on inputs and will give corresponding output whenever you will ask any question to it. After testing the above website you will come to know that it is not accurate because to train a ML model you require thousands and millions of data. The more data you will provide to the machine the better it can predict the output.
