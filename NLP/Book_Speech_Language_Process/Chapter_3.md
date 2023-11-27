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



# N-gram Language Models

Predicting is difficult—especially about the future, as the old quip goes. But how about predicting something that seems much easier, like the next few words someone is going to say? What word, for example, is likely to follow

```
Please turn your homework ...
```

Hopefully, most of you concluded that a very likely word is 'in', or possibly 'over', but probably not 'refrigerator' or 'the'. we will formalize this intuition by introducing models that assign a probability to each possible next
word. The same models will also serve to assign a probability to an entire sentence.

Probabilities are essential in any task in which we have to identify words in noisy, ambiguous input, like speech recognition. For a speech recognizer to realize that you said I will be back soonish and not I will be bassoon dish, it helps to know that back soonish is a much more probable sequence than bassoon dish.

For writing tools like spelling correction or grammatical error correction, we need to find and correct errors in writing like Their are two midterms, in which There was mistyped as Their, or Everything has improve, in which improve should have been improved. The phrase There are will be much more probable than Their are, and has improved than has improve, allowing us to help users by detecting and correcting these errors.

Models that assign probabilities to sequences of words are called **language models or LMs**. In this chapter we introduce the simplest model that assigns probabilities to sentences and sequences of words, the n-gram. An n-gram is a sequence of n words: a 2-gram (which we’ll call bigram) is a two-word sequence of words like “please turn”, “turn your”, or ”your homework”, and a 3-gram (a trigram) is a three-word sequence of words like “please turn your”, or “turn your homework”.

## N-Grams

Let’s begin with the task of computing P(w|h), the probability of a word w given some history h. Suppose the history h is “its water is so transparent that” and we want to know the probability that the next word is the :

$$ P(the|its water is so transparent that) $$

One way to estimate this probability is from relative frequency counts: take a very large corpus, count the number of times we see its water is so transparent that, and count the number of times this is followed by the. This would be answering the question “Out of the times we saw the history h, how many times was it followed by the word w”, as follows :

$$ P(the|\text{its water is so transparent that}) =
\frac {C(\text {its water is so transparent that the})}
{C(\text{its water is so transparent that})} $$

While this method of estimating probabilities directly from counts works fine in many cases, it turns out that even the web isn’t big enough to give us good estimates in most cases. 

For this reason, we’ll need to introduce more clever ways of estimating the probability of a word w given a history h, or the probability of an entire word sequence W. Let’s start with a little formalizing of notation. To represent the probability of a particular random variable X<sub>i</sub> taking on the value “the”, or P(X<sub>i</sub> = “the”), we will use the simplification P(the). We’ll represent a sequence of n words either as w<sub>1</sub> ...w<sub>n</sub> or w<sub>1:n</sub> (so the expression w<sub>1:n-1</sub> means the string w<sub>1</sub>,w<sub>2</sub>,...,w<sub>n-1</sub>). For the joint probability of each word in a sequence having a particular value P(X<sub>1</sub> = w<sub>1</sub>,X<sub>2</sub> = w<sub>2</sub>,...,X<sub>n</sub> = w<sub>n</sub>) we’ll use P(w<sub>1</sub>,w<sub>2</sub>,...,w<sub>n</sub>).

Now, how can we compute probabilities of entire sequences like P(w<sub>1</sub>,w<sub>2</sub>,...,w<sub>n</sub>)? One thing we can do is decompose this probability using the chain rule of probability :

$$ 
P(X_1 ... X_n) = P(X_1)P(X_2|X_1)P(X_3|X_{1:2})...P(X_n|X_{1:n-1}) \\
= \prod_{k=1}^{n} P(X_k|X_{1:k-1})
 $$
 
Applying the chain rule to words, we get :

$$ 
P(w_{1:n}) = P(w_1)P(w_2|w_1)P(w_3|w_{1:2})...P(w_n|w_{1:n-1}) \\
= \prod_{k=1}^{n} P(w_k|w_{1:k-1})
 $$

But using the chain rule doesn’t really seem to help us! We don’t know any way to compute the exact probability of a word given a long sequence of preceding words. The intuition of the n-gram model is that instead of computing the probability of a word given its entire history, we can approximate the history by just the last few words. The **bigram** model, for example, approximates the probability of a word given all the previous words P(w<sub>n</sub>|w<sub>1:n-1</sub>) by using only the conditional probability of the preceding word P(w<sub>n</sub>|w<sub>n-1</sub>). In other words, instead of computing the probability :

P(the|Walden Pond’s water is so transparent that) 

we approximate it with the probability :

P(the|that)

When we use a bigram model to predict the conditional probability of the next word, we are thus making the following approximation :

$$ 
P(w_n|w_{1:n−1}) ≈ P(w_n|w_{n−1})
 $$
 
The assumption that the probability of a word depends only on the previous word is called a **Markov** assumption. Markov models are the class of probabilistic models that assume we can predict the probability of some future unit without looking too far into the past. We can generalize the bigram (which looks one word into the past) n-gram to the trigram (which looks two words into the past) and thus to the n-gram (which looks n−1 words into the past). Let’s see a general equation for this n-gram approximation to the conditional probability of the next word in a sequence. We’ll use N here to mean the n-gram size, so N = 2 means bigrams and N = 3 means trigrams. Then we approximate the probability of a word given its entire context as follows :

$$
P(w_n|w_{1:n−1}) ≈ P(w_n|w_{n−N+1:n−1})
$$

An intuitive way to estimate probabilities is called maximum likelihood estimation or MLE. We get the MLE estimate for the parameters of an n-gram model by getting counts from a corpus, and normalizing the counts so that they lie between 0 and 1. For example, to compute a particular bigram probability of a word w<sub>n</sub> given a previous word w<sub>n-1</sub>, we’ll compute the count of the bigram C(w<sub>n-1</sub>w<sub>n</sub>) and normalize by the sum of all the bigrams that share the same first word w<sub>n-1</sub> :

$$
P(w_n|w_{n−1}) = \frac {C(w_{n−1}w_n)}{C(w_{n-1}w)} \\
= \frac {C(w_{n−1}w_n)}{C(w_{n-1})}
$$

Let’s work through an example using a mini-corpus of three sentences. We’ll first need to augment each sentence with a special symbol <s> at the beginning of the sentence, to give us the bigram context of the first word. We’ll also need a special end-symbol. </s>

```
<s> I am Sam </s>
<s> Sam I am </s>
<s> I do not like green eggs and ham </s>
```

Here are the calculations for some of the bigram probabilities from this corpus :

$$
P(I|<s>) = \frac{2}{3} = .67 \\ 
P(Sam|<s>) = \frac{1}{3} = .33 \\
P(am|I) = \frac{2}{3} = .67 \\
P(</s>|Sam) = \frac{1}{2} = 0.5 \\
P(Sam|am) = \frac{1}{2} = .5 \\ 
P(do|I) = \frac{1}{3} = .33
$$

Let’s move on to some examples from a slightly larger corpus than our 14-word example above. We’ll use data from the now-defunct Berkeley Restaurant Project, a dialogue system from the last century that answered questions about a database of restaurants in Berkeley. Below table shows bigram grammer of few words :

| | i | want | to | eat | chinese | food | lunch | spend |
|:-------------|:------------------|:--|:--|:--|:--|:--|:--|:--|
| i | 5 | 827 | 0 | 9 | 0 | 0 | 0 | 2 |
| want | 2 | 0 | 608 | 1 | 6 | 6 | 5 | 1 |
| to | 2 | 0 | 4 | 686 | 2 | 0 | 6 | 211 |
| eat | 0 | 0 | 2 | 0 | 16 | 2 | 42 | 0 |
| chinese | 1 | 0 | 0 | 0 | 0 | 82 | 1 | 0 |
| food | 15 | 0 | 15 | 0 | 1 | 4 | 0 | 0 |
| lunch | 2 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| spend | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |

After normalizing (dividing each cell by the appropriate unigram for its row) :

| | i | want | to | eat | chinese | food | lunch | spend |
|:-------------|:------------------|:--|:--|:--|:--|:--|:--|:--|
| i | 0.002 | 0.33 | 0 | 0.0036 | 0 | 0 | 0 | 0.00079 |
| want | 0.0022 | 0 | 0.22 | 0.0011 | 0.0065 | 0.0065 | 0.0054 | 0.0011 |
| to | 0.00083 | 0 | 0.0017 | 0.28 | 0.00083 | 0 | 0.0025 | 0.087 |
| eat | 0 | 0 | 0.0027 | 0 | 0.021 | 0.0027 | 0.056 | 0 |
| chinese | 0.0063 | 0 | 0 | 0 | 0 | 0.52 | 0.0063 | 0 |
| food | 0.014 | 0 | 0.014 | 0 | 0.00092 | 0.0037 | 0 | 0 |
| lunch | 0.0059 | 0 | 0 | 0 | 0 | 0.0029 | 0 | 0 |
| spend | 0.0036 | 0 | 0.0036 | 0 | 0 | 0 | 0 | 0 |

Here are a few other useful probabilities :

$$
P(i|<s>) = 0.25 \\ 
P(english|want) = 0.0011 \\
P(food|english) = 0.5 \\
P(</s>|food) = 0.68
$$

Now we can compute the probability of sentences like "I want English food" :

$$
P(<s> \text{i want english food} </s>) \\
= P(i|<s>)P(want|i)P(english|want)
P(food|english)P(</s>|food) \\
= .25×.33×.0011×0.5×0.68 \\
= .000031
$$

We always represent and compute language model probabilities in log format as log probabilities. Since probabilities are (by definition) less than or equal to 1, the more probabilities we multiply together, the smaller the product becomes. Multiplying enough n-grams together would result in numerical underflow. By using log probabilities instead of raw probabilities, we get numbers that are not as small.

## Evaluating Language Models