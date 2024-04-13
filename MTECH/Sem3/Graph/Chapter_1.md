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



# Introduction

With the rise of social media, the web has become a vibrant and lively realm in which billions of individuals all around the globe interact, share, post, and conduct numerous daily activities. Social media enables us to be connected and interact with each other anywhere and anytime – allowing us to observe human behavior in an unprecedented scale with a new lens.

This new social media world has no geographical boundaries and incessantly churns out oceans of data. As a result, we are facing an exacerbated problem of big data – “drowning in data, but thirsty for knowledge.” Can data
mining come to the rescue? Unfortunately, social media data is significantly different fromthe traditional data that we are familiar with in data mining.

## What is Social Media Mining

Social media shatters the boundaries between the real world and the virtual world. We can now integrate social theories with computational methods to study how individuals (also known as social atoms) interact and how communities (i.e., social molecules) form. Social Media Mining is the process of representing, analyzing, and extracting actionable patterns from social media data.

Social media mining cultivates a new kind of data scientist who is well versed in social and computational theories, specialized to analyze recalcitrant social media data, and skilled to help bridge the gap from what we know (social and computational theories) to what we want to know about the vast social media world with computational tools.

## New Challenges for Mining

Social media mining is an emerging field where there are more problems than ready solutions. Mining social media data is the task of mining user-generated content with social relations. This data presents novel challenges encountered in social media mining.

1. Big Data Paradox - Social media data is undoubtedly big. However, when Big Data we zoom into individuals for whom, for example, we would like to make Paradox relevant recommendations, we often have little data for each specific individual.
2. Obtaining Suffcient Samples - One of the commonly used methods to collect data is via application programming interfaces (APIs) from social media sites. Only a limited amount of data can be obtained daily. Without knowing the population’s distribution, how can we know that our samples are reliable representatives of the full data?
3. Noise Removal Fallacy - By its nature, social media data can contain a large portion of noisy data. For this data, we notice two important observations: (1) blindly removing noise can worsen the problem stated in the big data paradox because the removal can also eliminate valuable information, and (2) the definition of noise becomes complicated and relative because it is dependent on our task at hand.
4. Evaluation Dilemma - Astandard procedure of evaluating patterns in data Evaluation mining is to have some kind of ground truth. For example, a dataset can be Dilemma divided into training and test sets. Evaluating patterns from social media mining poses a seemingly insurmountable challenge.