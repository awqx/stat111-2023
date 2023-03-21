---
layout: post
title: Lecture 15 - Regression, hypothesis testing
date: 2023-03-21
lecture: 15
---

## Announcements

- Midterms available during lecture
- Can return midterms for regrade until **this Thursday**
- In this lecture
	- Descriptive regression
	- Hypothesis testing
- Coming up later
	- Bayesian statistics

## Descriptive regression

The standard setup is $(X_i, Y_i)$ are i.i.d. pairs. Components are dependent but the pairs are independent

When conditioning on our data $X$, our model becomes a lot more sensical because we can use prediction based on our current data. Getting the joint distribution of $Y$ is usually unhelpful. 

- Note some "non-math" counterintuitive results
	- If we have $y = \theta x$, we cannot necessary infer $x = \theta^{-1} y$
	- More on this in HW 6!

Let error $U = Y - \mathbb{E}[Y \mid X] = Y - \theta X$

- We can never observe the error $Y - \theta X$ b/c $\theta$ is an estimand
- We usually model or estimate the error using the residuals $Y - \hat\theta X$
- Do not confuse errors and residuals!
	- This results in category error

### Method of moments estimator

We can use Adam's law to get $\mathbb{E} [U]$ as

$$
\begin{aligned}
\mathbb E[U] &= \mathbb E[ \mathbb E[U | X]] \\
&= \mathbb E [\mathbb E[Y | X] - \theta X]] \\
&= \mathbb E[0] = 0.
\end{aligned}
$$

We also have

$$
\begin{aligned}
UX &= XY - \theta X^2 \\
&\to \mathbb E[UX] = \mathbb E[XY] - \theta \mathbb E[X^2]\\
\mathbb E[UX] &= \mathbb E[\mathbb E[UX | X]] \\
&= \mathbb E[X 0] = 0,
\end{aligned}
$$

which implies

$$
\theta = 
\frac{\mathbb E[XY]}{\mathbb E[X^2]}
$$

The previous result gives us an MoM estimator (not necessarily the MLE) of

$$
\hat\theta = 
\frac{\sum_{j = 1}^n X_j Y_j}{\sum_{j = 1}^n X_j^2}
$$

The above is also the MLE only in the case of Gaussian assumptions about the model. 

Is $\hat\theta$ unbiased?

We can also write $Y_j = U_j + \theta X_j$, giving us

$$
\hat\theta = 
\frac{\sum_{j = 1}^n X_j (U_j + \theta X_j)}{\sum_{j = 1}^n X_j^2} = 
\theta + 
\frac{\sum_{j = 1}^n X_j U_j}{\sum_{j = 1}^n X_j^2},
$$

which shows that the estimator is unbiased if the second term is 0. Using Adam's Law, this can be shown, as we already found that $\mathbb E [UX] = 0$. 

- We can also show the estimator is consistent, or converges in probability
	- We want to show that the second term converges to $0$ in probability
	- Use LLN on the top (remember to multiply by 1) and then use Slutsky's 

$$
\frac{\sum_{j = 1}^n X_j U_j}{\sum_{j = 1}^n X_j^2} = 
\frac{ \frac{1}{n} \sum_{j = 1}^n X_j U_j}
{\frac{1}{n} \sum_{j = 1}^n X_j^2} 
\overset{p}{\to} 
\frac{\mathbb E [X_1 U_1]}{\mathbb E[X_1^2]} = 0
$$

What is the asymptotic distribution? We have

$$
\sqrt{n} (\hat\theta - \theta) = 
\sqrt{n} \frac{ \frac{1}{n} \sum_{j = 1}^n X_j U_j}
{\frac{1}{n} \sum_{j = 1}^n X_j^2} 
\overset{d}{\to}
\mathcal{N}
\left(
0, 
\frac{\textrm{Var}(X_1 U_1)}{(\mathbb E[X_1^2])^2}
\right).
$$

The above also follows from LLN and Slutsky's. 

## Hypothesis testing

Joe generally dislikes hypothesis testing. Usually, Bayesian methods or making a confidence interval are more informative and assumptions are clearer. 

Hypothesis testing and use of $p$-values, unfortunately, are very widely used in the sciences. Often, the use of hypothesis testing is done recklessly (or purposefully misused) to find significant results in an experiment. 

### Setting of hypothesis testing

Let $\Theta$ be the parameter space (capital theta). The parameter space contains our possible values of $\theta$. $\Theta$ can have any dimension, as appropriate to the problem. We want to test the **null hypothesis** (denoted $H_0$) that $\theta \in \Theta_0$ versus the **alternate hypothesis** (denoted $H_1$) that $\theta \in \Theta_1$. 

Normally, we assume that $\Theta_0, \Theta_1$ form a partition of $\Theta$. Some usage does not require this, which is strange because it would leave part of the parameter space uncovered. 

- Definition of partition (in this small case)
	- $\Theta = \Theta_0 \cup \Theta_1$
	- $\Theta_0 \cap \Theta_1 = \varnothing$

Although this setup looks like we're deciding between $H_0$ and $H_1$, this is not how we're reasoning through hypotheses. For example, scientists might say they "reject" or "fail to reject" the null rather than "reject/accept" (in this class, you can say "retain" instead of "fail to reject"). If you're interested in making decisions under uncertainty, check out game theory or decision theory. 

### Neyman-Pearson framework

This is the most common use of hypothesis testing, so you often do not need to declare you're using the Neyman-Pearson framework while performing hypothesis testing. 

- **Type I error**: false positive
	- Reject the null when it is true
- **Type II error**: false negative
	- Retain the null when it is false

> ...
> 
> (Bonus stats joke)
> 
> A statistician is someone who wants to be wrong exactly 5% of the time. 
> 
> ...

The framework is as follows:

1. Fix $\alpha \in (0, 1)$
2. We want $\Pr(\textrm{reject } H_0 \mid H_0) \leq \alpha$
	1. Type I error

- Typically, $\alpha = 0.05$
- Why? No very robust reasons
	- About the complement of the area covered by $\pm 2$ standard deviations from the mean in a Normal distribution
	- Nice number

Note that the framework only focuses on Type I error and $\alpha$. 

Type II error is often important, too. We could also consider situations when we would like to minimize $\Pr(\textrm{retain }H_0 \mid H_1)$. 

- **Power function**: returns the probability that we reject the null given that the true parameter is $\theta$, or $\beta(\theta) = \Pr(\textrm{reject } H_0 \mid \theta)$
	- We want power to be large when $\theta \in \Theta_1$ and low when $\theta \in \Theta_0$
		- $\beta(\theta) \leq \alpha$ when $\theta \in \Theta_0$ by definition
		- Assuming we're still in the Neyman-Pearson framework
	- Confusingly, $\beta$ is often used as the parameter of interest in regression

When testing, we create a test statistic $T$, where we reject $H_0$ if $\mid T \mid > c$ and retain $H_0$ if $\mid T \mid < c$. 

- $c$ is the **critical value** of the test.

What do we do when $T$ is discrete and $T = c$? To be consistent with the framework, we would have to randomly choose with certain probability (not necessarily a fair probability) to keep Type I error rate less than $\alpha$. 

We often redefine retention to be $\mid T \mid \leq c$ to avoid the above problem. 

#### Simple and composite null

- **Simple null**: $\Theta_0$ contains only one element
	- I.e., $\Theta_0 = \theta_0$
	- Very rarely should be literally interpreted as equality in practical science
	- E.g., if testing for difference in average test scores between two groups, whether they're *exactly* the same is often not important, as only a large difference would be a cause for concern
	- Thinking of the null hypothesis as $H_0: \mid \theta - \theta_0 \mid \leq \epsilon$ may be more useful
- **Composite null**: $\Theta_0$ is a set with more than one element
	- E.g., greater than or less than null hypotheses
- Aside about data collection and significance
	- If you collect enough data, you can often reject the null due to spurious correlations
	- The field of psychology warns researchers against these statistically significant but incidental artifacts in large data sets

### Solving for the critical value

Solving for $c$ is a fairly standard STAT 110 problem. We only need to find $c$ such that $\Pr(\mid T \mid > c \mid \theta_0) = \alpha$. 

**Ex**: One sample $t$-test. 

Let $Y_j$ be i.i.d. Normals with mean $\mu$ and variance $\sigma^2$. We have $H_0: \mu = \mu_0$ and $H_1: \mu \neq \mu_0$. 

We can use $T = \sqrt{n}\frac{\bar Y - \mu_0}{\hat\sigma}$ as our test statistic. This is a standard Normal under the null. Intuitively, if $\bar Y$ is far from $\mu_0$ (and $T$'s magnitude is large), it would seem like the null should be rejected. 

Under the null hypothesis, $T \sim t_{n - 1}$ ($t$ distribution with $n - 1$ degrees of freedom). We can write $1 - \alpha = \Pr(\mid T \mid \leq c)$. Because of the symmetry of $t_{n - 1}$ and our null, we can write 

$$
\alpha = P(\mid T \mid \geq c) = 2 P(T > c) \to 1 - \frac{\alpha}{2} = \Pr(T \leq c),
$$

where $c$ is easy to find from the quantile function of the $t_{n - 1}$ distribution. 