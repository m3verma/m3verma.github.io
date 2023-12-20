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



# Naive Bayes and Sentiment Classification

Classification lies at the heart of both human and machine intelligence. Deciding what letter, word, or image has been presented to our senses, recognizing faces or voices, sorting mail, assigning grades to homeworks; these are all examples of assigning a category to an input. The goal of classification is to take a single observation, extract some useful features, and thereby classify the observation into one of a set of discrete classes.

We focus on one common text categorization task, sentiment analysis, the ex- sentiment analysis traction of sentiment, the positive or negative orientation that a writer expresses toward some object. A review of a movie, book, or product on the web expresses the author’s sentiment toward the product, while an editorial or political text expresses sentiment toward a candidate or political action. Spam detection is another important commercial application, the binary classification task of assigning an email to one of the two classes spam or not-spam. Many lexical and other features can be used to perform this classification.

## Naive Bayes Classifiers

We introduce the multinomial naive Bayes classifier, so called because it is a Bayesian classifier that makes a simplifying (naive) assumption about how the features interact. We represent a text document bag of words as if it were a bag of words, that is, an unordered set of words with their position ignored, keeping only their frequency in the document. For example instead of representing the word order in all the phrases like “I love this movie” and “I would recommend it”, we simply note that the word I occurred 5 times in the entire excerpt, the word it 6 times, the words love, recommend, and movie once, and so on.

Naive Bayes is a probabilistic classifier, meaning that for a document d, out of all classes c ∈ C the classifier returns the class ^c which has the maximum posterior probability given the document.

$$
\hat c = argmaxP(c|d)
$$

Using Bayes' rule :

$$
P(x|y)=\frac{P(y|x)P(x)}{P(y)} \\
\hat c = argmax \frac{P(d|c)P(c)}{P(d)}
$$

We can conveniently simply by dropping the denominator P(d). This is possible because we will be computing it for each possible class. But P(d) doesn't change for each class; we are always asking about the most likely class for same document d.

$$
\hat c = argmax \frac{P(d|c)P(c)}{}
$$

To return to classification: we compute the most probable class c given some document d by choosing the class which has the highest product of two probabilities : the prior probability of the class P(c) and the likelihood of the document P(d|c). Without loss of generalization we can represent a document d as a set of features :

$$
\hat c = argmax \frac{P(f_1,f_2...,f_n)P(c)}{}
$$

Naive Bayes classifiers make two simplifying assumptions :

1. The first is the bag-of-words assumption discussed intuitively above: we assume position doesn’t matter, and that the word “love” has the same effect on classification whether it occurs as the 1st, 20th, or last word in the document. Thus we assume that the features f<sub>1</sub>, f<sub>2</sub>,..., f<sub>n</sub> only encode word identity and not position.
2. The second is commonly called the naive Bayes assumption: this is the conditional independence assumption that the probabilities `P(fi|c)` are independent given
the class c and hence can be ‘naively’ multiplied.

$$
c_{NB} = argmax P(c) \prod P(f|c)
$$

Naive Bayes calculations, like calculations for language modeling, are done in log space, to avoid underflow and increase speed.

$$
c_{NB} = argmax logP(c) \sum logP(w_i|c)
$$

## Training the Naive Bayes Classifier




