---
title:  "The Importance of Importance Sampling"
excerpt: "An important precursor to understanding Markov Chain Monte Carlo based sampling methods."
search: false
tags: 
  - Sampling
categories:
  - Sampling Methods
last_modified_at: 2018-07-29T08:06:00-05:00
comments: true
# classes: single
classes: wide
# toc: true
# toc_sticky: true
---

# Background
Sampling is widely used across many fields, but, in the context of deep learning it is used to estimate, for example, gradient of a cost functional, the partition function to normalize a probability distribution specified by a Markov network.

Monte Carlo Sampling
When a sum or integral is intractable to compute, we resort to Monte Carlo sampling to sample from a probability distribution instead and consider the integral as an expectation over that distribution. We can then find an empirical average over the samples to approximate the sum. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-monte-carlo.png){: .align-center}

But what if it's infeasible to sample from p?

# Importance Sampling
To solve the above problem, let's analyze the equation first. What part of the equation should act as p and what part should be f? The answer is we don't know. Why? Because it can always be written as:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-importance-sampling.jpg){: .align-center}

Now, our p becomes q and f becomes pf/q. We can now sample from q and approximate pf/q. Not knowing the decomposition is a problem but the good news is that in many cases, we already know both p and f. Now, how do we know the number of samples we need to sample from q to obtain good accuracy over our approximation?

## Optimal Importance Sampling
To find an answer, we convert a Monte Carlo estimator to an importance sampling estimator like below:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-importance-monte.jpg){: .align-center}

As we can see below, the expected value of this estimator is unbiased! Which is good because now we can choose any q and sample from it. However, it has some variance!

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-is-bias-variance.jpg){: .align-center}

The variance will be minimum when q takes an optimal value of:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-optimal-q.jpg){: .align-center}

Theoretically, when q is optimal, we need only a single sample to approximate the integral of f. But, this is not practical as solving for q* essential solves our original problem! And this is infeasible. So we choose a q such that we reduce the variance somewhat.

## Biased Importance Sampling
We have seen that the optimal importance sampling technique required us to normalize q by finding Z which is intractable. But the advantage of that technique is that its expected value was unbiased. We can trade-off between this and find a biased q that does not requires normalization:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/is/eq-biased-importance-sampling.jpg){: .align-center}

But, as we can see above, if the number of samples that we draw from q reaches infinity, the estimator becomes unbiased! So, we keep sampling as many samples as we can from q to get as unbiased estimate as we can and we don't need to normalize.

## Curse of dimensionality in important sampling
Consider the variance of the importance sampling estimator above. If the q in denominator gets very large or very small, and the numerator pf does not compensates for that, the variance either becomes very small, in this case the samples collected are useless, or it becomes very large (this is rare). This occurs because q is chosen to be a very simple distribution (because we need to continuously sample from it) and when x is high dimensional, the dynamic range of joint probabilities can be very large.

To solve the problems of importance sampling, we "approximately sample" from the distribution p using Markov chains. 

### Next steps
I strongly recommend that you watch [this](https://www.youtube.com/watch?v=TNZk8lo4e-Q) three lecture series on Importance Sampling and Markov Chain Monte Carlo by Prof. Nando De Freitas from the University of British Columbia! 

# References

All of the above content, unless specified otherwise, is heavily inspired, directly or indirectly, from the [Deep Learning book](https://www.deeplearningbook.org/) by Ian Goodfellow, Yoshua Benjio and Aaron Courville. The online version of the book is available for free and I highly recommend that you read it!
{: .notice}