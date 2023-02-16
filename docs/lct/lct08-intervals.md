---
layout: post
title: Lecture 08 - Confidence intervals
date: 2023-02-16
lecture: 8
---

- How do we get intuition about the Fisher?
- Can we see what happens when Fisher info is large or small?

**Ex** (Fisher with Normal). $Y \sim \mathcal{N}(\theta, \sigma^2)$, where $\sigma^2$ is known. The derivation of the likelihood and log-likelihood follow from previous examples of the Normal. We have that the score (first derivative of log-likelihood) and second derivative are

$$
\ell'(\theta; y) = \frac{y - \theta}{\sigma^2}, \quad \ell''(\theta) = -\frac{1}{\sigma^2}
$$

So, taking the negative expectation of the latter, we have that fisher info is inversely proportional to $\sigma^2$. 

- With large $\sigma^2$, there is little info and vice versa for small $\sigma^2$. 
- If we were to plot the log-likelihood, we would see low curvature around the MLE when $\sigma^2$ is large
	- Versus very peaked for low $\sigma^2$

**Ex** (German tank problem). Observed data is the observed serial numbers of the tanks. We want the total number of tanks, $t$. Assume our observed serial numbers are a simple random sample. 

Our likelihood function is:

$$
L(t; y) = 
\frac{1}{\binom{t}{n}}, \textrm{  assuming that  } y_1, \dots, y_n \in \{1, \dots, t\}
$$

Because the above is zero otherwise, we can write this using indicators and order statistics as

$$
L(t; y) = 
\frac{\mathbb{1}(y_{(n)} \leq t)}{\binom{t}{n}}
$$

- Plotting the likelihood function, the support depends on $t$, which is an irregularity. 
- We can still *calculate* the MLE, but nice properties don't follow. 
- The MLE is $y_{(n)}$, the highest observed serial number. 
- Is this estimator biased?
- We'll get the expectation using LOTP
	- This is isomorphic to Joe's favorite Stat 110 problem: the Feynman restaurant problem!
		- Found in Ch. 4
	- $P(Y_{(n)} = m) = \frac{\binom{m - 1}{n - 1}}{\binom{t}{n}}$ for $t \geq m \geq n$
	- So we can find the expectation as follows

$$
\mathbb{E}[Y_{(n)}]
= 
\frac{1}{\binom{t}{n}}
\sum_{m = n}^t 
m \binom{m - 1}{n - 1} = \frac{n}{n + 1}(t + 1).
$$

- This estimator is biased, but the bias does not depend on $t$. So we can correct the bias by defining a new estimator $\frac{n + 1}{n} Y_{n} - 1$ 
- In practice:
	- Spies estimated 1400 tanks
	- Statisticians estimated 256 tanks

## Interval estimation

**Def** (Confidence interval). Let $L(\mathbf{Y}), U(\mathbf{Y})$ be functions of the data such that $L(\mathbf{y}) \leq U(\mathbf{y})$ for all $\mathbf{y}$. The random interval $[L(\mathbf{Y}), U(\mathbf{Y})]$ is a $1 - \alpha$ confidence interval (CI) if $P(L(\mathbf{Y}) \leq \theta \leq U(\mathbf{Y}); \theta) = 1 - \alpha$ where $\alpha \in (0, 1)$ is fixed. 

We often use $\alpha = 0.05$ in science. The LHS of the probability expression is often called the **coverage probability**. 

**Def** (Pivot). A **pivot** is a quantity with a known distribution, but we usually cannot calculate the pivot from the data. 

- Contrast a pivot with a statistic, where the latter is calculated from a data

**Ex** (Making a CI). Let $\hat\theta \sim \mathcal{N}(\theta, \sigma^2/n)$ where $\sigma^2$ is known. We have that

$$
\frac{\hat\theta - \theta}{\sigma / \sqrt n} \sim \mathcal{N}(0, 1). 
$$

The above (LHS) is a pivot.

So, we can create a 95% CI using the pivot, where $Q$ is the standard Normal quantile function.

$$
P\left(Q(0.025) \leq \frac{\hat\theta - \theta}{\sigma / \sqrt n}  \leq Q(0.975)\right) = 0.95.
$$

We can rearrange to find the interval, where

$$
P\left(
\hat\theta -
\frac{\sigma Q(0.025)}{\sqrt{n}} \leq \theta \leq \hat\theta +
\frac{\sigma Q(0.025)}{\sqrt{n}}
\right) = 0.95,
$$

so the endpoints of our CI are $\hat\theta \pm \frac{\sigma 1.96}{\sqrt{n}}$.

- Sanity check for intervals
	- It should shrink as $n$ gets large

**Ex**. Suppose we have observations $y_1, \dots, y_n$ from $Y_1, \dots, Y_n \overset{\text{i.i.d.}}{\sim} \mathcal N(\mu, \sigma^2)$, with both parameters unknown. What is the distribution of $\hat\sigma^2$? We have

$$
\hat\sigma^2 = 
\frac{1}{n - 1}
\sum_{j = 1}^n
(Y_j - \bar Y)^2, \quad
\frac{n - 1}{\sigma^2} \hat\sigma^2 \sim \chi^2_{n - 1}.
$$

The LHS is a pivot. (Also, note that the $\chi^2_m$ distribution is a special case of the Gamma). 

(Also see Ex. 10.4.3 in the Stat 110 textbook for the proof of this distribution). 