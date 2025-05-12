---
layout: post
title: Perhaps the Simplest Introduction of Adversarial Examples Ever
date: 2018-08-21 00:00:00
description:
tags: 
categories: 
---
Original published in [Medium](https://medium.com/data-science/perhaps-the-simplest-introduction-of-adversarial-examples-ever-c0839a759b8d)

# Perhaps the Simplest Introduction of Adversarial Examples Ever

In this article, we will understand the intuition of adversarial examples and how we could create them with Python.

At  the time of writing, I just delved into the formula and tried to code it riding on the logistic regression sample I created for my previous article ([Link](https://medium.com/@kentsui/what-is-logistic-regression-451858f73bcb)).

So, I could assure you my introduction is simple, and very basic. Yet the intuition here could be extended to higher dimension and more complex models like deep learning.

## Everything Starts with the Simplest Data

![](https://miro.medium.com/v2/resize:fit:1122/1*rSy21wE5dP2s-c00paX6Ew.png)

Going back to the easy problem raised in my first article, if we want to distinguish the red and blue classes, we could immediately know that the  **decision boundary** should be right in the middle of the two clusters, like the below. With that in place, any new data falling in the top right region would then be classified as blue and those falling in the left bottom region would then be classified as red.

![](https://miro.medium.com/v2/resize:fit:1092/1*fTXVRNyHcMKAC6hRFivWmg.png)

**Adversarial Examples**  are the well designed samples with an intention to cause machine learning to make mistake. How can we cause the purple line to make error  **if we cannot move the purple line**?

It turns out that it is not difficult at all, we just need to move the sample to the opposite region so that the blue point falls into the red region, and the red point falls in the blue region. Not a surprise at all.

![](https://miro.medium.com/v2/resize:fit:1098/1*HTdV-MemOWMu3Vg17Q459Q.png)

![](https://miro.medium.com/v2/resize:fit:1400/1*AOBaZ2OCfBFMUfolD0T4YA.png)

Epsilon is a small number controlling the size of adversarial attack, which needs to be chosen in order to be effective but not to be too obvious.

With the increase of epsilon, we can foresee the decision boundary cannot work out. The logistic regression model error grows from 2.9% to 7.6% when epsilon becomes 0.2, and to 22.6% when epsilon becomes 0.5.

## Fast Gradient Sign Method (FGSM)

What was graphically displayed above is actually using FGSM.

![](https://miro.medium.com/v2/resize:fit:1008/1*5TpmLkAiGXdiUUXjjlPSTw.png)

![](https://miro.medium.com/v2/resize:fit:1400/1*8GU23C25xIJGxNu7-OqxGQ.png)

In essence,  FGSM is to  add the noise (not random noise) whose direction is the same as the gradient of the cost function with respect to the data. The noise is scaled by epsilon, which is usually constrained to be a small number via max norm. The magnitude of gradient does not matter in this formula, but the direction (+/-).

## Translating into Python Code

FGSM can be implemented in Python in a few lines.

In gradient descent, the gradient of cost with respect to weight is (Y_Prediction — Y_True)**X**.

In adversarial examples, the gradient of cost with respect to data is (Y_Prediction — Y_True)**W**.

The little change means we can  recycle the code of gradient calculation by just replacing data with weight.

![](https://miro.medium.com/v2/resize:fit:718/1*3dMC5jLIaetPLXVkNXF_8A.png)

## Intuition and Takeaway

Below are the questions that I asked myself when I tried to make sense of the formula.

**Intuition 1: Why is the formula related to gradient of the cost function?**

To increase  model error means increasing the cost function.

**Intuition 2: It looks like Gradient Descent Optimization. Isn’t it?**

Gradient descent is to update the weight of the model so as to minimize cost function via obtaining gradient with respect to the weight.  **I****n this case, as we cannot change the weight, in order to increase the cost, the only way is to change the data.**

In some sense, we are doing gradient ascent of the cost function with respect to the data to increase the cost, so as to increase the model error.

**Intuition 3: Can adversarial examples help increase the quality of logistic regression? How can logistic regression defend such attack?**

From my intuition, using logistic regression cannot avoid such attack. As seen in the graph, no matter where you put the purple line, the error will still stay high as the adversarial examples makes two clusters getting blurred.  However, adversarial example shall be extremely useful to help non-linear model build defense against these attacks.

The full python code can be found at this  [link](https://github.com/kenhktsui/adversarial_examples).

PS: I am not an expert in this field, but definitely an enthusiast. This article is not technical but I hope the intuition derived from the simplest case can be extended to all cases. Please clap if this introduction helps!

