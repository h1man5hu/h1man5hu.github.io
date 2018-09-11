---
title:  "Maximum Likelihood Estimation vs Maximum A Posteriori Estimation"
excerpt: "Comparing the frontiers of Frquentist and Bayesian Statistical approaches to find the best estimators for machine learning models."
search: false
tags: 
  - Maximum Likelihood Estimation
  - Maximum a Posteriori Estimation
categories:
  - Machine Learning
last_modified_at: 2018-03-10T08:06:00-05:00
comments: true
# classes: single
classes: wide
# toc: true
# toc_sticky: true
---

# Maximum Likelihood Estimation (MLE)
MLE is a principle from which we can derive specific functions that are good estimators for different models. MLE is based on the frequentist approach to statistics, in the sense that the true value of the statistic to estimate is treated as fixed but unknown. 

## Estimators
We call the fixed value of the parameter as a point estimate. A good point estimate is the one which has low bias and variance. An estimator is unbiased if it's expected value over the parameters is equal to the true value. For example, sample mean is an unbiased estimator of the Gaussian mean parameter. But, sample variance is not. To get an unbiased estimator of the sample variance:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-variance-estimator.jpg){: .align-center}

Variance of an estimator is simply the expected value of the squared sampling deviations. It measures how the value of the statistic varies per sample in the dataset. In general, variance decreases as the number of samples increases. This is a desired property for a good estimator and is called consistency of the estimator.

## Bias-Variance Trade-off 
If both bias and variance are not low, as is desired, what should we prefer? How do we compare two models with different bias and variance? There are two ways:
	1. Perform cross-validation
	2. Use mean squared error (MSE)
The MSE can be decomposed into bias and variance:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-mse-bv.jpg){: .align-center}

Thus, evaluating MSE incorporates both the bias and the variance. An estimator which has low MSE has both the bias and variance low.

## Maximum Likelihood Estimator
The estimator is given by:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-mle.jpg){: .align-center}

Let's simplify it further:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-mle-ce.jpg){: .align-center}

Thus, maximizing the likelihood is the same as minimizing the cross-entropy of the model's distribution given by:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-ce.jpg){: .align-center}

If you don't know what cross-entropy is, check out my blog post about it here. Whatever we choose, the optimal θ will be the same but the values of the objective functions will be different. Usually, even Maximum Likelihood is converted to Minimizing the negative log-likelihood (NLL), log to prevent underflow. So, it’s either minimizing NLL or the cross-entropy. 
MLE, thus, can be seen as a principle to search for good estimators, having low bias and variance, bringing the model's distribution closer to the true data generating process distribution by minimizing the cross-entropy between them.


# Maximum A Posteriori (MAP) Estimation 
## Bayesian Statistics
The Bayesian view of statistics is a very powerful tool to measure degree of belief. It is one of the four interpretations of probability: classical, frequentist, propensity and subjective (Bayesian). The internet is filled with articles and videos on "Frequentist vs Bayesian approach" of probability. Make sure to check them out and that you understand the Bayes' theorem well before proceeding. The main advantage of Bayesian view is that we can incorporate our prior beliefs about the statistic to estimate into our probabilistic model. This, combined with the likelihood, gives us the required posterior distribution over the statistical parameter. 

## Machine learning under Bayesian View
Contrary to the frequentist approach of MLE, the dataset is not treated as a random variable. Instead, the parameters of the model are represented as random variable while the true parameter values are treated as uncertain. A very simple prior p(θ) such as a uniform or a Gaussian distribution is chosen. This reduces the variance but increases the bias. The observation of the data (likelihood) then causes the posterior to concentrate around the values specified by the prior. Applying the Bayes' theorem on our model we get:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-bayes.jpg){: .align-center}

Bayesian methods typically generalize much better when limited training data is available, but typically suffer from high computational cost when the number of training examples is large.
{: .notice--danger}

## MAP Estimator
Using Bayes' theorem, we can find the posterior probability of the parameters of our model. But for most deep probabilistic models, calculating this is intractable, mostly due to the marginalization involved in finding the value of the denominator (normalizer). Hence, we revert can back to finding the point estimate using MLE. But what if we still want to take the advantage of adding prior beliefs to the model provided by Bayesian view? We find the MAP point estimate (the point of maximal posterior probability)! The MAP estimator is given by:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/mle_map/eq-map.jpg){: .align-center}

If we take the prior as a Gaussian distribution, it's log is proportional to the weight decay term. The first part can be seen as maximizing the conditional log-likelihood. Hence, MAP in this case corresponds to L2 regularization. MAP is thus used as a straightforward way to design complicated regularization terms e.g. by using a mixture of Gaussian distributions. 
{: .notice--info}

# References
All of the above content, unless specified otherwise, is heavily inspired, directly or indirectly, from the [Deep Learning book](https://www.deeplearningbook.org/) by Ian Goodfellow, Yoshua Benjio and Aaron Courville. The online version of the book is available for free and I highly recommend that you read it!
{: .notice}