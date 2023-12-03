---
layout: post
title: Explaining cross-entropy to teenagers
tags: ML, teaching
---

In Ukraine, we have been in a state of war for almost two years now, and I'm becoming seriously concerned about the state of science in our country. My mom is a math school teacher, and she once suggested that I come and give a short introductory talk about how logarithmic functions are used in training AI. This way, kids can have a better understanding of how the knowledge they are acquiring is used in building modern machine learning algorithms.

I realized that explaining cross-entropy loss would be a good starting point to capture the curiosity of young minds and demonstrate a specific use case for the new concept they are learning, namely logarithmic functions.

I came across a thread about KL divergence on HN. The top comment caught my attention because it explained KL divergence from a bottom-up approach, introducing concepts of surprise, expected surprise, and expected surprise of the other person, and gradually delving into KL divergence. Since the focus of this write-up is cross-entropy, I'll stop there.

<div class="row justify-content-sm-center">
    <div class="col-sm-4">
        {% include figure.html path="assets/img/unfair-die.jpeg" title="image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    weird unfair die, created with chatGPT
</div>
So, I took a similar methodology. First, I needed to introduce what probability is. Using a model of a rolling die, I started to ask questions about the probabilities of different events. Then we began to reason together on how to express the degree of surprisal, ensuring it can't be negative and should increase as the probability of a particular event decreases. This led us to: $$ -\log p(x), \quad 0 < x \leq 1 $$. 

Later, we arrived at the conclusion that the expected surprise for a random variable with an even probability distribution is equal to the surprise of any individual event. However, it gets more interesting when the probability density function (pdf) is not even anymore. Eventually, we discussed an unfair die, or a weird die that, for some reason, has the value of 4 on multiple faces. Now, our task is to measure the expected surprise of another person whose belief about the unfair die $$q$$ is different from the actual one $$p$$. This led us to cross-entropy, which expresses the expected surprise of a person or neural network whose beliefs differ from the actual ones: $$ H (p,q) = - \sum p(x) \log q(x) $$

So, to summarize, cross-entropy is used to measure how well predictions match real data distribution. The lower the cross-entropy, the better the model of the data distribution we have at our hands.

I created short lecture [notes]({{ site.url }}/assets/pdf/logarithms-in-ml.pdf) for this introduction, it's written in Ukrainian using amazing [typst](https://typst.app/).

