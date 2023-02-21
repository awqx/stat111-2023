---
layout: post
title: Lecture 09 - Cauchy, Student t, Sufficiency
date: 2023-02-21
lecture: 9
---

## Review
**Ex** (Normal pivot, 95% CI). By CLT, we have $\hat\theta \sim \mathcal{N}(\theta, s^2)$. We can create a pivot $\displaystyle \frac{\hat\theta - \theta}{s} \sim \mathcal{N}(0, 1)$. By properties of $\Phi$, we can write $\Pr\left(-1.96 \leq \frac{\hat\theta - \theta}{s} \leq 1.96 \right) \approx 0.95$. So, out confidence interval is $\hat\theta \pm 1.96 s$. However, we likely do not know $s$. So, we can write the approximate CI $\hat\theta \pm 1.96 \hat s$ to create our interval estimator.  

Don't worry too much about the number of approximations. 

- Many CIs look like the point estimate $\pm 1.96$ times the standard error (or the point estimate of the SE)
- Generally, use $1.96$ instead of $2$ b/c it's clearer you're using the Normal distribution
	- Use to have its own Wikipedia page
	- Now renamed as 97.5th percentile point. [See here](https://en.wikipedia.org/wiki/97.5th_percentile_point)

## Misinterpretation of CIs

- Survey of psych students often misinterpret the 95% CI $[0.1, 0.4]$ for a mean parameter
	- Hoekstra, R., Morey, R.D., Rouder, J.N. _et al._ Robust misinterpretation of confidence intervals. _Psychon Bull Rev_ **21**, 1157–1164 (2014). https://dx.doi.org/10.3758/s13423-013-0572-3

Statement | First Years (_n_ = 442) | Master Students (_n_ = 34) | Researchers (_n_ = 118)
---|---|---|---
_The probability that the true mean is greater than 0 is at least 95 %_ | 51 % | 32 % | 38 %
_The probability that the true mean equals 0 is smaller than 5 %_ | 55 % | 44 % | 47 %
_The “null hypothesis” that the true mean equals 0 is likely to be incorrect_ | 73 % | 68 % | 86 %
_There is a 95 % probability that the true mean lies between 0.1 and 0.4_ | 58 % | 50 % | 59 %
_We can be 95 % confident that the true mean lies between 0.1 and 0.4_ | 49 % | 50 % | 55 %
_If we were to repeat the experiment over and over, then 95 % of the time the true mean falls between 0.1 and 0.4_ | 66 % | 79 % | 58 %

- All the interpretations in the table are WRONG
- The interval is random, not the true mean!
- Confidence intervals are like ring toss
	- [Read more](https://medium.com/@EpiEllie/having-confidence-in-confidence-intervals-8f881712d837)


## Distributions in statistics

### Evil Cauchy distribution
- Heavy-tailed distribution
	- Otherwise looks similar to the Normal distribution
- Joe has an evil Cauchy distribution plush in his office

**Def** (Cauchy distribution). The Cauchy distribution has PDF $f(x) = \frac{1}{\pi (1 + x^2)}$. By representation, if $Z_1, Z_2$ are i.i.d. standard Normal, then $Z_1 / Z_2 \sim \textrm{Cauchy}$

- Let $C \sim \textrm{Cauchy}$
- The mean does not exist
	- This seems unintuitive b/c $\Pr(Z_2 = 0) = 0$ and b/c the distribution is symmetric
	- However, what happens is $\infty - \infty$
		- Use definition of expectation on RHS: $\int_0^\infty \frac{x}{1 + x^2} dx$
		- Integral is infinite
			- Compare it w/ $\int_1^\infty \frac{dx}{x}$, which is known to diverge
			- Can also use $u$-substitution of $1 + x^2 = u$
- The Cauchy distribution also does not work with CLT
	- The sample mean of Cauchy distributions has the same distribution as a Cauchy distribution
	- I.e., for $C_1, \dots, C_n$ i.i.d., we have that $\bar C = \frac{1}{n} \sum_{j = 1}^n C_j \sim C_1$

### Student $t$-distributions
- Covered in Ch. 10 of Stat 110 textbook
	- Not thoroughly covered otherwise
	- Seems unmotivated without statistical inference

**Def** (Student $t$-distribution). The PDF of the Student $t$-distribution with $n$ degrees of freedom is

$$
f_T(t) = \frac{\Gamma((n + 1)/2)}{\sqrt{n \pi} \Gamma(n / 2)}
(1 + t^2 / n)^{-(n + 1) / 2}.
$$

The Cauchy is a Student $t$-distribution with $n = 1$. 

By representation, if $T$ is a Student $t$-distribution with $n$ degrees of freedom, 

$$
T = \frac{Z}{\sqrt{V / n}}: \quad Z \sim \mathcal{N}(0, 1), \;\; V \sim \chi^2_n, \;\; Z \perp V.
$$

- Hey, that looks sort of like the operations we did on our pivot earlier!
- $t$-distribution allows for construction of pivot when standard deviation unknown
- $t$-distribution introduced by Gosset under the pseudonym "Student"
	- William Sealy Gosset was chief brewer for Guinness
	- Gosset used the distribution to conduct quality control for Guinness
- Gosset motivated to characterize the distribution to create a pivot when we use sample mean and sample variance

**Ex** (Motivating example for $t$-distribution). $Y_1, \dots, Y_n \sim \mathcal{N}(\mu, \sigma^2)$, where observations are i.i.d. and the mean and variance are unknown. We have $\hat\mu = \bar Y \sim \mathcal{N}(\mu, \sigma^2 / n)$. When using the estimate of $\sigma$, we have $\frac{\bar Y - \mu}{\hat\sigma / \sqrt{n}}$. 

We know $\bar Y$ has the same distribution as $\frac{\sigma}{\sqrt n} Z$, where $Z$ is standard Normal. We also have that $\bar Y \perp \hat\sigma$. How do we show this independence? This is a special property of the Normal distribution. For reference, the estimate is below:

$$
\hat\sigma^2 = \frac{1}{n - 1} \sum_{j = 1}^n (Y_j - \bar Y)^2
$$

(Proof is in Ex. 7.5.9 in the Stat 110 book). We can then write

$$
\frac{\bar Y - \mu}{\hat\sigma / \sqrt{n}} \sim 
\left(\frac{\sigma}{\sqrt n} Z\right) \bigg/ 
\left( \frac{\sigma}{\sqrt n} \sqrt{\frac{V_{n - 1}}{n - 1}} \right) = 
\frac{Z}{\sqrt{\frac{V_{n - 1}}{n - 1}}} \sim t_{n - 1}
$$

- Check this with CMT practice
- If we have $T_n \sim t_{n}$, we have that as $n$ grows large, $T_n \to Z$ as $n \to \infty$
	- Denominator tends to $\sqrt{V_n / n} \to 1$
	- Then use Slutsky

**Ex** ($t$-distribution pivot). 

$$
0.95 = 
\Pr\left(
a \leq \frac{\bar Y - \mu}{\hat\sigma / \sqrt{n}} \leq b
\right)
$$

We can now get $a, b$ using the `R` command `qt`. We can find `qt(0.975, n - 1)` for $b$ and then take the negative for $a$ due to symmetry. So, $\bar Y \pm q \hat\sigma / \sqrt{n}$ is our CI, where $q$ is calculated with `R`. The interval should be wider than the standard Normal. 

Here are some examples of degrees of freedom $n$ and the corresponding $q$ values for 95% CIs. 

n | q
---|---
10 | 2.26
50 | 2.01
474 | 1.96


> Hack for creating 95% confidence intervals! Roll a 20-sided die. If you roll 20, your CI can be $\varnothing$. The other 19/20 numbers, your CI is $-\infty, \infty$. This is why we need to be conscientious of the practicality of our CI. 

## Sufficiency

**Def** (Sufficient statistic). Let our estimand be $\theta$ and the data be $\mathbf Y$. A statistic $\mathbf{T}$ is **sufficient** if the conditional distribution of $\mathbf Y \mid \mathbf T$ is free of $\theta$. 

**Thm** (Factorization criterion). $T$ is a sufficient statistic $\iff$ $f_\theta(y) = g_\theta(t) h(y)$, where $f$ is the PDF. As the notation suggests, $g_\theta$ can also involve $\theta$. We cannot have $\theta$ in $h(y)$. 

- We found a 2-dimensional sufficient statistic for viral spread in HW 1

**Ex** (Poisson sufficient statistic). $Y_j \overset{iid}{\sim} \textrm{Pois}(\lambda)$. Let $T = \sum_{j = 1}^n Y_j$. We want to show $T$ is sufficient. 

- It's easier to use the factorization criterion than the definition of sufficiency
- We can write the PDF as

$$
\prod_{j = 1}^n \frac{e^{-\lambda} \lambda^{y_j}}{y_j!} = \frac{e^{-n \lambda} \lambda^t}{\prod_{j = 1}^n y_j!}: g_\theta(t) = e^{-n \lambda} \lambda^t, h(y) = \frac{1}{\prod_{j = 1}^n y_j!}.
$$

- *Intuition*: The sufficient statistic is the representation of the data we need in order to recover the likelihood function
	- $h(y)$ is droppable in the likelihood expression

**Ex** (Sufficient statistic of the Normal). $Y_j \overset{i.i.d.}{\sim} \mathcal{N}(\mu, \sigma^2)$ with both parameters unknown. We can have many different sufficient statistics, such as $(\sum_j Y_j, \sum_j Y_j^2)$ and $(\bar Y, \frac{1}{n} \sum_j (Y_j - \bar Y)^2)$. We can use the factorization criterion and the sum of squares decomposition to write

$$
\left(\frac{1}{\sigma \sqrt 2 \pi}\right)^n
\exp\left\{
-\frac{1}{2 \sigma^2}
\left( 
\sum_j (y_j - \bar y)^2 
+ n(\bar y - \mu)^2
\right)
\right\}.
$$