---
title:  "Understanding the mathematics of weight decay"
excerpt: "Let's dive deep into how adding a single term to a cost function causes some weights to shrink to 0."
search: false
tags: 
  - Regularization
  - Weight Decay
categories:
  - Regularization
last_modified_at: 2018-06-17T08:06:00-05:00
comments: true
# classes: single
classes: wide
# toc: true
# toc_sticky: true
---

# Parameter Norm Penalties
Adding parameter norm penalties to a neural network's cost function is a common way to limit the capacity of the model, thus, adding a regularization effect. It works be reducing the value (norm) of the model's parameters. By the term "model parameters", in this context, we usually mean the weights and not the biases. Each weight observes interaction between two variables in a variety of conditions whereas each bias only controls a single variable and hence it requires less data to fit.  It is a common practice to only add penalties to weights and not the biases. It is often better to have a different penalty coefficient for each layer of a neural network but that would need a lot of correct hyperparameter search, therefore, we resort to a reasonable approach to using only one coefficient for whole neural network. There are two types of parameter norm penalties, L1 and L2, we focus on L2 in this post.

We can actually cause the weights to decay to any value we want, but since we don’t know whether the optimal values should be positive or negative, we let it decay to zero.
{: .notice}

Regularizing bias parameters can induce significant underfitting!
{: .notice--danger}

# L2 Parameter Regularization
L2 parameter regularization, also known as weight decay, ridge regression or Tikhonov regularization, is the more commonly used of the two parameter norm penalties. The term "weight decay" implies that it the weights are decayed down to the origin. 

## Intuition
Let us observe the gradient of the regularized cost function below:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-gradient-regularized.jpg){: .align-center}

We can now see how a regularized gradient step will look like:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-gradient-step.jpg){: .align-center}

Clearly, the addition of the weight decay term has modified the learning rule to multiplicatively shrink the weight vector by a constant factor on each step, just before performing the usual gradient update. Now, let's see how this step affects the training.

## 1. Finding the optimal weights for unregularized cost
Assume that w* represents the optimal parameter weights and we approximate the unregularized cost around it. The approximation is given by:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-approximation.jpg){: .align-center}

Since w* is optimal, the gradient of the cost w.r.t. the weights will be zero at this point. In other words, H is a positive semidefinite matrix. The gradient is given by:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-gradient-hessian.jpg){: .align-center}

## 2. Analyzing the effect of weight decay on the above condition
Assume that w˜ represents the regularized weights. Adding weight decay to the minimum cost condition we get:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-add-decay.PNG){: .align-center}

As is clear above, if the regularization coefficient approaches zero, the regularized weight parameters approach the unregularized optimal weights. Since H is positive semidefinite, it is also real and symmetric, this means that we can perform eigendecomposition on it:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/eq-conclusion.jpg){: .align-center}

# Conclusion
From the last equation we can observe that the eigenvectors and eigenvalues of the Hessian of the unregularized cost function define the regularized weight parameters. Specifically, along the directions where:
* The eigenvalues of H are relatively larger than the regularization coefficient (J is reduced significantly along this direction), the effect of regularization is small, the weights along these directions remain almost unchanged
* The eigenvalues of H are relatively smaller than the regularization coefficient, the effect of regularization is huge and the weights of these directions are decayed to zero.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/weight_decay/gph-effect-l2.png){: .align-center}
*Taken from the Deep Learning Book, Page 233.*
{: style="text-align: center;"}

The eigenvalue of the eigenvector along the horizontal direction of the H of J is small and thus w* is shrunk a lot towards zero along this direction (w<sub>1</sub>), while in the vertical direction the eigenvalue is large and thus w<sub>2</sub> is shrunk a lot less.


# References
All of the above content, unless specified otherwise, is heavily inspired, directly or indirectly, from the [Deep Learning book](https://www.deeplearningbook.org/) by Ian Goodfellow, Yoshua Benjio and Aaron Courville. The online version of the book is available for free and I highly recommend that you read it!
{: .notice}