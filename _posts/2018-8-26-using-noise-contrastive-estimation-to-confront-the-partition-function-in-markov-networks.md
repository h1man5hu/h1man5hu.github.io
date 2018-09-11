---
title:  "Using Noise Contrastive Estimation to confront the partition function in Markov Networks"
excerpt: "An important technique to deal with the infamous partition function and also closely related to the key idea behind Generative Adversarial Networks (GANs)."
search: false
tags: 
  - Partition Function
  - Noise Contrastive Estimation
categories:
  - Partition Function
last_modified_at: 2018-08-26T08:06:00-05:00
comments: true
# classes: single
classes: wide
# toc: true
# toc_sticky: true
---

# Background
The probability distribution defined by an undirected graphical model (UGM) is unnormalized. To normalize it, we divide it by a constant called the partition function or Z. The calculation of Z, in most UGMs, is intractable since it involves marginalization over the complete probability distribution. This happens when Z becomes a function of the parameters. There are several techniques to confront this function. First, let's understand the log-likelihood gradient:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nce/eq-log-likelihood-gradient.jpg){: .align-center}

For the positive phase, we use approximate inference techniques like Expectation Maximization and Variational Inference whereas, for the negative phase, we use the techniques described below. There are following main ways to tackle the partition function:
1. Using pseudolikelihood or score-matching to train the model without computing anything related to Z
2. Using Markov Chain Monte Carlo based methods like Stochastic Maximum Likelihood (SML) or Contrastive Divergence (CD), and its variants like Persistent Contrastive Divergence (PCD) and Fast Persistent Contrastive Divergence (F-PCD) to estimate the gradient of logZ.
3. Using Noise Contrastive Estimation (NCE) to simultaneously learn model parameters and -logZ
4. Using Annealed Importance Sampling or Bridge Sampling to directly estimate Z.

# Noise Contrastive Estimation
NCE treats the negative phase as a parameter (c) and estimates both θ and c simultaneously using the same algorithm:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nce/eq-nce-approach.jpg){: .align-center}

This approach is not directly trainable with Maximum Likelihood Estimation (MLE) as that could maximize the likelihood of the data by just setting c to be very high. This, however, would not yield a valid probability distribution. To make it compatible with MLE, we need to convert this unsupervised problem to a supervised problem.

## Making NCE approach compatible with MLE
We take the model and add a new distribution to it called "noise distribution". These two are then combined together using a binary classifier to form a joint model distribution. We choose the noise distribution such that it is easy to evaluate and sample to from. Also, we define our new joint probability distribution such that there is an equal chance that we generate data sample from model distribution or noise distribution. Thus,:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nce/eq-joint.jpg){: .align-center}

We can now use MLE to fit pjoint to ptrain and find the optimal  θ and c together:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nce/eq-mle.jpg){: .align-center}

We can even see how this new joint distribution of the model is essentially logistic regression applied on the difference in the log probabilities of model and noise distributions: 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nce/eq-logistic.jpg){: .align-center}

As we have seen above, NCE is based on an idea that a good generative model should be able to distinguish data from noise. From this, we can also infer that a good generative model should be able to generate samples that no classifier can distinguish from data and this is the core key idea behind Generative Adversarial Networks!
{: .notice--success}

# References
All of the above content, unless specified otherwise, is heavily inspired, directly or indirectly, from the [Deep Learning book](https://www.deeplearningbook.org/) by Ian Goodfellow, Yoshua Benjio and Aaron Courville. The online version of the book is available for free and I highly recommend that you read it!
{: .notice}