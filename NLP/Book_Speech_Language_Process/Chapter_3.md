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

Let’s begin with the task of computing `P(w|h)`, the probability of a word w given some history h. Suppose the history h is “its water is so transparent that” and we want to know the probability that the next word is the :

$$ P(the|\text{its water is so transparent that}) $$

One way to estimate this probability is from relative frequency counts: take a very large corpus, count the number of times we see its water is so transparent that, and count the number of times this is followed by the. This would be answering the question “Out of the times we saw the history h, how many times was it followed by the word w”, as follows :

$$ P(the|\text{its water is so transparent that}) =
\frac {C(\text {its water is so transparent that the})}
{C(\text{its water is so transparent that})} $$

While this method of estimating probabilities directly from counts works fine in many cases, it turns out that even the web isn’t big enough to give us good estimates in most cases. 

For this reason, we’ll need to introduce more clever ways of estimating the probability of a word w given a history h, or the probability of an entire word sequence W. Let’s start with a little formalizing of notation. To represent the probability of a particular random variable X<sub>i</sub> taking on the value “the”, or P(X<sub>i</sub> = “the”), we will use the simplification P(the). We’ll represent a sequence of n words either as w<sub>1</sub>...w<sub>n</sub> or w<sub>1:n</sub> (so the expression w<sub>1:n-1</sub> means the string w<sub>1</sub>,w<sub>2</sub>,...,w<sub>n-1</sub>). For the joint probability of each word in a sequence having a particular value P(X<sub>1</sub> = w<sub>1</sub>,X<sub>2</sub> = w<sub>2</sub>,...,X<sub>n</sub> = w<sub>n</sub>) we’ll use P(w<sub>1</sub>,w<sub>2</sub>,...,w<sub>n</sub>).

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

But using the chain rule doesn’t really seem to help us! We don’t know any way to compute the exact probability of a word given a long sequence of preceding words. The intuition of the n-gram model is that instead of computing the probability of a word given its entire history, we can approximate the history by just the last few words. The **bigram** model, for example, approximates the probability of a word given all the previous words `P(w_n|w_{1:n-1})` by using only the conditional probability of the preceding word `P(w_n|w_{n-1})`. In other words, instead of computing the probability :

$$
P(the|\text{Walden Pond’s water is so transparent that}) 
$$

we approximate it with the probability :

$$
P(the|that)
$$

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
P(w_n|w_{n−1}) = \frac {C(w_{n−1}w_n)}{\sum_w C(w_{n-1}w)} \\
= \frac {C(w_{n−1}w_n)}{C(w_{n-1})}
$$

Let’s work through an example using a mini-corpus of three sentences. We’ll first need to augment each sentence with a special symbol &lts&gt at the beginning of the sentence, to give us the bigram context of the first word. We’ll also need a special end-symbol. &lt/s&gt

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

The best way to evaluate the performance of a language model is to embed it in an application and measure how much the application improves. Such end-to-end evaluation is called extrinsic evaluation. Unfortunately, running big NLP systems end-to-end is often very expensive. Instead, it would be nice to have a metric that can be used to quickly evaluate potential improvements in a language model. An intrinsic evaluation metric is one that measures the quality of a model independent of any application. So if we are given a corpus of text and want to compare two different n-gram models, we divide the data into training and test sets, train the parameters of both models on the training set, and then compare how well the two trained models fit the test set. 

But what does it mean to “fit the test set”? The answer is simple: whichever model assigns a higher probability to the test set—meaning it more accurately predicts the test set—is a better model. Since our evaluation metric is based on test set probability, it’s important not to let the test sentences into the training set. Sometimes we use a particular test set so often that we implicitly tune to its characteristics. We then need a fresh test set that is truly unseen. In such cases, we call the initial test set the development test set or, devset. In practice, we often just divide our data into 80% training, 10% development, and 10% test.

### Perplexity

The perplexity (sometimes called PPL for short) of a language model on a test set is the inverse probability of the test set, normalized by the number of words. For a test set W = w<sub>1</sub>w<sub>2</sub> ...w<sub>N</sub>,:

$$
Perplexity(W) = P(w_1 w_2 ... w_N )^{-1/N} \\
Perplexity(W) = \sqrt[N]{\frac {1}{P(w_1 w_2 ... w_N )}} \\
$$

Using Chain Rule 

$$
Perplexity(W) = \sqrt[N]{\prod_{1=1}^{N} \frac {1}{P(w_i|w_1 w_2 ... w_{i-1} )}}
$$

Thus, minimizing perplexity is equivalent to maximizing the test set probability according to the language model. There is another way to think about perplexity: as the weighted average branching factor of a language. The branching factor of a language is the number of possible next words that can follow any word. Consider the task of recognizing the digits in English (zero, one, two,..., nine), given that (both in some training set and in some test set) each of the 10 digits occurs with equal probability P = 1\10 . The perplexity of this mini-language is in fact 10.

But suppose that the number zero is really frequent and occurs far more often than other numbers. Let’s say that 0 occur 91 times in the training set, and each of the other digits occurred 1 time each. Now we see the following test set: 0 0 0 0 0 3 0 0 0 0. We should expect the perplexity of this test set to be lower since most of the time the next number will be zero, which is very predictable, i.e. has a high probability. Thus, although the branching factor is still 10, the perplexity or weighted branching factor is smaller. Perplexity can be used to compare different n-gram models :

| | Unigram | Bigram | Trigram |
|:--|:--|:--|:--|
| Perplexity | 962 | 170 | 109 |

As we see above, the more information the n-gram gives us about the word sequence, the higher the probability the n-gram will assign to the string. So a lower perplexity can tell us that a language model is a better predictor of the words in the test set.

## Sampling sentences from a language model

One important way to visualize what kind of knowledge a language model embodies is to sample from it. Sampling from a distribution means to choose random points according to their likelihood. Thus sampling from a language model—which represents a distribution over sentences—means to generate some sentences, choosing each sentence according to its likelihood as defined by the model.

A visualization of the sampling distribution for sampling sentences by repeatedly sampling unigrams. The blue bar represents the relative frequency of each word (we’ve ordered them from most frequent to least frequent, but the choice of order is arbitrary). The number line shows the cumulative probabilities. If we choose a random number between 0 and 1, it will fall in an interval corresponding to some word. The expectation for the random number to fall in the larger intervals of one of the frequent words (the, of, a) is much higher than in the smaller interval of one of the rare words (polyphonic).

## Generalization and Zeroes

The n-gram model, like many statistical models, is dependent on the training corpus. One implication of this is that the probabilities often encode specific facts about a given training corpus. Another implication is that n-grams do a better and better job of modeling the training corpus as we increase the value of N. To give an intuition for the increasing power of higher-order n-grams lets see below example :

```
1 gram :
Input – To him swallowed confess hear both. Which. Of save on trail for are ay device and rote life have
Output – Hill he late speaks; or! a more to leg less first you enter

2 gram :
Input – Why dost stand forth thy canopy, forsooth; he is this palpable hit the King Henry. Live king. Follow.
Output – What means, sir. I confess she? then all sorts, he is trim, captain.

3 gram :
Input – Fly, and will rid me these news of price. Therefore the sadness of parting, as they say, ’tis done.
Output – This shall forbid it should be branded, if renown made it empty.

4 gram :
Input – King Henry. What! I will go seek the traitor Gloucester. Exeunt some of the watch. A great banquet serv’d in;
Output – It cannot be but so
```

The longer the context on which we train the model, the more coherent the sentences. In the unigram sentences, there is no coherent relation between words or any sentence-final punctuation. The bigram sentences have some local word-to-word coherence. The trigram and 4-gram sentences are beginning to look a lot like Shakespeare. Indeed, a careful investigation of the 4-gram sentences shows that they look a little too much like Shakespeare. The words It cannot be but so are directly from King John. This is because, not to put the knock on Shakespeare, his oeuvre is not very large as corpora go (N = 884,647,V = 29,066), and our n-gram probability matrices are ridiculously sparse. There are V<sup>2</sup> = 844,000,000 possible bigrams alone, and the number of possible 4-grams is V<sup>4</sup> = 7×10<sup>17</sup>. Thus, once the generator has chosen the first 4-gram (It cannot be but), there are only five possible continuations (that, I, he, thou, and
so); indeed, for many 4-grams, there is only one continuation.

Statistical models are likely to be pretty useless as predictors if the training sets and the test sets are as different as Shakespeare and WSJ. How should we deal with this problem when we build n-gram models? One step is to be sure to use a training corpus that has a similar genre to whatever task we are trying to accomplish. To build a language model for a question-answering system, we need a training corpus of questions. Matching genres and dialects is still not sufficient. Our models may still be subject to the problem of sparsity. For any n-gram that occurred a sufficient number of times, we might have a good estimate of its probability. But because any corpus is limited, some perfectly acceptable English word sequences are bound to be missing from it. Consider the words that follow the bigram denied the in the WSJ Treebank3 corpus, together with their counts :

```
denied the allegations: 5
denied the speculation: 2
denied the rumors: 1
denied the report: 1
```

But suppose our test set has phrases like:

```
denied the offer
denied the loan
```

Our model will incorrectly estimate that the P(offer|denied the) is 0! These zeros—things that don’t ever occur in the training set but do occur in the test set—are a problem for two reasons. First, their presence means we are underestimating the probability of all sorts of words that might occur, which will hurt the performance of any application we want to run on this data. Second, if the probability of any word in the test set is 0, the entire probability of the test set is 0. What do we do about zeros?

### Unknown Words

What do we do about words we have never seen before? In most real situations, we have to deal with words we haven’t see OOV before, which we’ll call unknown words, or out of vocabulary (OOV) words. The percentage of OOV words that appear in the test set is called the OOV rate. One way to create an open vocabulary system is to model these potential unknown words in open vocabulary the test set by adding a pseudo-word called <UNK>.

There are two common ways to train the probabilities of the unknown word model <UNK>. The first one is to turn the problem back into a closed vocabulary one by choosing a fixed vocabulary in advance :

1. Choose a vocabulary (word list) that is fixed in advance.
2. Convert in the training set any word that is not in this set (any OOV word) to the unknown word token <UNK> in a text normalization step.
3. Estimate the probabilities for <UNK> from its counts just like any other regular word in the training set.

The second alternative, in situations where we don’t have a prior vocabulary in advance, is to create such a vocabulary implicitly, replacing words in the training data by <UNK> based on their frequency. For example we can replace by <UNK> all words that occur fewer than n times in the training set, where n is some small number, or equivalently select a vocabulary size V in advance (say 50,000) and choose the top V words by frequency and replace the rest by <UNK>. In either case we then proceed to train the language model as before, treating <UNK> like a regular word.

## Smoothing

What do we do with words that are in our vocabulary but appear in a test set in an unseen context? To keep a language model from assigning zero probability to these unseen events, we’ll have to shave off a bit of probability mass from some more frequent events and give it to the events we’ve never seen. This modification is called smoothing or discounting.

### Laplace Smoothing

The simplest way to do smoothing is to add one to all the n-gram counts, before we normalize them into probabilities. All the counts that used to be zero will now have a count of 1, the counts of 1 will be 2, and so on. This algorithm is called Laplace smoothing. Laplace smoothing does not perform well enough to be used in modern n-gram models. Recall that the unsmoothed maximum likelihood estimate of the unigram probability of the word w<sub>i</sub> is its count c<sub>i</sub> normalized by the total number of word tokens N :

$$
P(w_i) = \frac{c_i}{N}
$$

Laplace smoothing merely adds one to each count. Since there are V words in the vocabulary and each one was incremented, we also need to adjust the denominator to take into account the extra V observations.

$$
P_{Laplace}(w_i) = \frac{c_i+1}{N+V}
$$

Instead of changing both the numerator and denominator, it is convenient to describe how a smoothing algorithm affects the numerator, by defining an adjusted count c<sup>∗</sup>. This adjusted count is easier to compare directly with the MLE counts and can be turned into a probability like an MLE count by normalizing by N.

$$
c_{i}^{*} =(c_i+1) \frac{N}{N+V}
$$

A related way to view smoothing is as discounting (lowering) some non-zero counts in order to get the probability mass that will be assigned to the zero counts. Thus, instead of referring to the discounted counts c<sup>∗</sup>, we might describe a smoothing algorithm in terms of a relative discount d<sub>c</sub>, the ratio of the discounted counts to the original counts :

$$
d_c = \frac{c^*}{c}
$$

Let’s smooth our Berkeley Restaurant Project bigrams :

| | i | want | to | eat | chinese | food | lunch | spend |
|:-------------|:------------------|:--|:--|:--|:--|:--|:--|:--|
| i | 6 | 828 | 1 | 10 | 1 | 1 | 1 | 3 |
| want | 3 | 1 | 609 | 2 | 7 | 7 | 6 | 2 |
| to | 3 | 1 | 5 | 687 | 3 | 1 | 7 | 212 |
| eat | 1 | 1 | 3 | 1 | 17 | 3 | 43 | 1 |
| chinese | 2 | 1 | 1 | 1 | 1 | 83 | 1 | 1 |
| food | 16 | 1 | 16 | 1 | 2 | 5 | 1 | 1 |
| lunch | 3 | 1 | 1 | 1 | 1 | 2 | 1 | 1 |
| spend | 2 | 1 | 2 | 1 | 1 | 1 | 1 | 1 |

For add-one smoothed bigram counts, we need to augment the unigram count by the number of total word types in the vocabulary V :

$$
P_{Laplace}(w_n|w_{n−1}) = \frac {C(w_{n−1}w_n)+1}{\sum_w (C(w_{n-1}w)+1)} =  \frac {C(w_{n−1}w_n)+1}{C(w_{n-1})+V}
$$

Thus, each of the unigram counts given in the previous section will need to be augmented by V = 1446.

| | i | want | to | eat | chinese | food | lunch | spend |
|:-------------|:------------------|:--|:--|:--|:--|:--|:--|:--|
| i | 0.0015 | 0.21 | 0.00025 | 0.0025 | 0.00025 | 0.00025 | 0.00025 | 0.00075 |
| want | 0.0013 | 0.00042 | 0.26 | 0.00084 | 0.0029 | 0.0029 | 0.0025 | 0.00084 |
| to | 0.00078 | 0.00026 | 0.0013 | 0.18 | 0.00078 | 0.00026 | 0.0018 | 0.055 |
| eat | 0.00046 | 0.00046 | 0.0014 | 0.00046 | 0.0078 | 0.0014 | 0.02 | 0.00046 |
| chinese | 0.0012 | 0.00062 | 0.00062 | 0.00062 | 0.00062 | 0.52 | 0.0012 | 0.00062 |
| food | 0.0063 | 0.00039 | 0.0063 | 0.00039 | 0.00079 | 0.002 | 0.00039 | 0.00039 |
| lunch | 0.0017 | 0.00056 | 0.00056 | 0.00056 | 0.00056 | 0.0011 | 0.00056 | 0.00056 |
| spend | 0.0012 | 0.00058 | 0.0012 | 0.00058 | 0.00058 | 0.00058 | 0.00058 | 0.00058 |

### Add-k smoothing

One alternative to add-one smoothing is to move a bit less of the probability mass from the seen to the unseen events. Instead of adding 1 to each count, we add a fracadd-k tional count k (.5? .05? .01?). This algorithm is therefore called add-k smoothing.

$$
P_{Add-k}^{*}(w_n|w_{n−1}) = \frac {C(w_{n−1}w_n)+k}{C(w_{n-1}) + kV}
$$

### Backoff and Interpolation

There is an additional source of knowledge we can draw on. In other words, sometimes using less context is a good thing, helping to generalize more for contexts that the model hasn’t learned much about. There are two ways to use this n-gram “hierarchy”. In backoff, we use the trigram if the evidence is sufficient, otherwise we use the bigram, otherwise the unigram. In other words, we only “back off” to a lower-order n-gram if we have zero evidence for a higher-order interpolation n-gram. By contrast, in interpolation, we always mix the probability estimates from all the n-gram estimators, weighting and combining the trigram, bigram, and unigram counts. In simple linear interpolation, we combine different order n-grams by linearly interpolating them.

$$
\hat P(w_n|w_{n-2}w_{n-1}) = \lambda_1P(w_n)+\lambda_2P(w_n|w_{n-1})+\lambda_3P(w_n|w_{n-2}w_{n-1}) \\
where \sum_i\lambda_i=1
$$

Both the simple interpolation and conditional interpoheld-out lation λs are learned from a held-out corpus. A held-out corpus is an additional training corpus, so-called because we hold it out from the training data, that we use to set hyperparameters like these λ values. We do so by choosing the λ values that maximize the likelihood of the held-out corpus. In a backoff n-gram model, if the n-gram we need has zero counts, we approximate it by backing off to the (n-1)-gram. We continue backing off until we reach a history that has some counts. In order for a backoff model to give a correct probability distribution, we have to discount the higher-order n-grams to save some probability mass for the lower order n-grams. This kind of backoff with discounting is also called Katz backoff. In Katz backoff we rely on a discounted probability P<sup>∗</sup> if we’ve seen this n-gram before. Otherwise, we recursively back off to the Katz probability for the shorter-history (n-1)-gram. The probability for a backoff n-gram P<sub>BO</sub> is thus computed as follows :

$$
P_{BO}(w_n|w_{n-N+1:n-1}) = 
\begin{cases}
P^*(w_n|w_{n-N+1:n-1}), & \text{if C($w_{n-N+1:n}$)>0} \\
\alpha(w_{n-N+1:n-1})P_{BO}(w_n|w_{n-N+2:n-1}), & \text{Otherwise.}
\end{cases}
$$

## Huge Language Models and Stupid Backoff

By using text from the web or other enormous collections, it is possible to build extremely large language models. Efficiency considerations are important when building language models that use such large sets of n-grams. Rather than store each word as a string, it is generally represented in memory as a 64-bit hash number, with the words themselves stored on disk. Probabilities are generally quantized using only 4-8 bits, and n-grams are stored in reverse tries. An n-gram language model can also be shrunk by pruning, for example only storing n-grams with counts greater than some threshold or using entropy to prune less-important n-grams. With very large language models a much simpler algorithm may be sufficient. The algorithm is called stupid backoff. Stupid backoff gives up the idea of trying to make the language model a true probability distribution. There is no discounting of the higher-order probabilities. If a higher-order n-gram has a zero count, we simply backoff to a lower order n-gram, weighed by a fixed weight.

$$
S(w_i|w_{i-N+1:i-1}) = 
\begin{cases}
\frac{count(w_{i-N+1:i})}{count(w_{w-N+1:i-1})} & \text{if count($w_{i-N+1:i}$)>0} \\
\lambda S(w_i|w_{i-N+2:i-1}) & \text{Otherwise.}
\end{cases}
$$
