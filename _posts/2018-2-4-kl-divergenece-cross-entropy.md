---
title:  "Kullback-Leibler Divergence and Cross-Entropy"
excerpt: "Did you know that the cross-entropy loss actually came from the branch of Information Theory? No? Let's explore!"
search: false
tags: 
  - KL Divergence
  - Cross-Entropy
  - Loss function
  - Information Theory
categories:
  - Probability and Information Theory
last_modified_at: 2018-02-04T08:06:00-05:00
# header:
#   image: "assets/images/claudeshannon.jpg"
#   teaser: "assets/images/claudeshannon.jpg"
comments: true
# classes: single
classes: wide
# toc: true
# toc_sticky: true
---

Kullback-Leibler Divergence, specifically its commonly used form cross-entropy is widely used as a loss functional throughout deep learning. In this post, we will look at why is it so useful and the intuition and history behind it. But, first we need to have a basic understanding of the Information Theory.

# Information Theory: An Introduction
In 1948, Claude E. Shannon in his paper "A Mathematical Theory of Computation"<sup>[1]</sup> proposed a mathematical theory to find fundamental limits on signal processing and communication operations. It has had a crucial impact on several fields and finds applications from data compression to the missions to explore space! The field is huge and cannot be covered in this post. However, we will discuss the key ideas from the information theory that are used in deep learning. 
Information theory takes the fact that learning that an unlikely event has occurred is more informative than learning that a likely event has occurred and helps us in quantifying how much information is present in a signal.
Formally, this quantity is called self-information.

# Self-information
Self-information is defined as:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/kl_ce/eq-self-information.png){: .align-center}

The units depend on the nature of the logarithm, it can be nats (base e) or shannon (base 2). Note that self-information is defined over an event not a probability distribution. Let's plot the graph:

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/kl_ce/gph-self-information.png){: .align-center}
*Graphed using Desmos*
{: style="text-align: center;"}

As we can see, it satisfies the core idea of information theory described above. But how do we find self-information over a complete distribution?

# Shannon Entropy
Self-information over a distribution is called Shannon entropy. It is calculated by simply taking expectation of self-information over a distribution. In other words, it is the expected amount of information in an event drawn from a distribution.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/kl_ce/gph-shannon.png){: .align-center}
*This plot shows how distributions that are closer to deterministic have low Shannon entropy while distributions that are close to uniform have high Shannon entropy. Taken from the Deep Learning Book, Page 75.*
{: style="text-align: center;"}

# KL Divergence
Relative Shannon entropy of two distributions is called KL divergence. It is defined as:
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/kl_ce/kl.png){: .align-center}
Since Shannon entropy gives the expected amount of information in a signal, KL divergence can be seen as the extra amount of information we will need to send a message containing symbols drawn from probability distribution P, when we use a code that was designed to minimize the length of messages drawn from probability distribution Q. The most important property of KL divergence is that it is always greater than zero. Together these two properties make it perfect to be used as a loss function for a machine learning model! This brings us to cross-entropy.

## Cross-entropy
Out of all the terms we discussed above, this is the one you might probably be most aware of. In machine learning, we optimize our loss function to bring our model's probability distribution close to the true data generating distribution. We don't change the data distribution, our training data remains the same throughout the optimization process, it's just out model's distribution that changes. This means that we only need to optimize the KL divergence with respect to our model's distribution and not the data distribution! Therefore, the Shannon entropy of data generating distribution remains constant in KL divergence and as we do not care about the exact value of the divergence, we just want to minimize it, we can omit it from the equation and we get the cross-entropy loss for our model:
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/kl_ce/eq-cross-entropy.png){: .align-center}
Cross-entropy loss is also known as negative log-likelihood as is clear from the formula above.

# Conclusion
To train a machine learning model, we need to minimize the difference between the model's probability distribution and the true data generating process's probability distribution. We can think of the difference in terms of "Information Gain". If the information gain is zero, both distributions should be same. If there is a lot of information gain, the distributions must be very different and the difference (loss) should be higher. Relative Shannon entropy or KL divergence provides a measure for this gain (or loss). Optimizing the model to reduce this quantity will bring our model's distribution closer to the data distribution. We don't change the data distribution hence we omit it from KL divergence and get our cross-entropy loss.

### Next steps
Do checkout Luis Serrano's [YouTube video](https://www.youtube.com/watch?v=9r7FIXEAGvs) and [Medium blog post](https://medium.com/udacity/shannon-entropy-information-gain-and-picking-balls-from-buckets-5810d35d54b4) on "Shannon Entropy and Information Gain". He provides excellent intuition behind advanced topics like this one using straightforward real-world examples.

# References

All of the above content, unless specified otherwise, is heavily inspired, directly or indirectly, from the [Deep Learning book](https://www.deeplearningbook.org/) by Ian Goodfellow, Yoshua Benjio and Aaron Courville. The online version of the book is available for free and I highly recommend that you read it!
{: .notice}

1. [Shannon, C.E. (1948), "A Mathematical Theory of Communication", Bell System Technical Journal, 27, pp. 379–423 & 623–656, July & October, 1948.](https://dl.acm.org/citation.cfm?id=584093)
2. [https://en.wikipedia.org/wiki/Information_theory](https://en.wikipedia.org/wiki/Information_theory)