---
layout: post
title: Lecture 14 - Post-midterm, regression
date: 2023-03-09
lecture: 14
---
## Midterm details
- For midterm regrade requests
	- Get your physical midterm
	- Write your request on the cover
	- Turn in to Joe
		- During class or OH
	- Do **NOT** modify your work
		- Violation of Honor Code
		- The EV of this is very negative
	- Turn in before March 23rd (Thursday)
- Five-number summary of midterm
	- Mean 71, SD 19, quantiles are (58, 75, 85)
- Go to Joe's OH to get midterm if you couldn't pick it up in class
- Solutions posted on Canvas
- Overall course cutoffs
	- >85: at least an A-, barring extreme circumstances
	- 65-85: B level
	- 50-64: C level
	- <50: D level

## Midterm review
### Bird chirps
- Note that most important regularity condition is violated
	- Support depends on the parameter
- Need indicator or some other method for working with violations of the support
- Don't put the indicator in the exponent!
	- Results in $0^0$, for example
	- Indicator should be scalar
		- If there is at least one violation of the support, then the parameter is not valid
- Density category error
	- $Y$ is continuous, so use PDF not probability
	- $P(Y = y)$ is not valid!
	- Illustrative case: $X \sim \textrm{Expo}(1), Y = 2X$
		- $P(Y = y) = P(2X = y) = P(X = y/2) = e^{-y/2}$
			- The last thing integrates to $2$, which is not a probability and results in $0 = 0 = 0 = 2$
		- The above can use a CDF
		- $P(Y \leq y) = P(X \leq y/2) = 1 - e^{-y/2}$, which is correctly normalized
- Statistic or estimator must be function of the data
	- "Bigly = big and ugly", used to describe a mess
	- Do *not* put parameters into statistics
	- Confidence interval must also be statistics

### Gamma CI
- Sanity checks for confidence intervals
	- Confidence interval should get narrower as sample size grows
	- Confidence interval should depend on data
- Limit category error
	- If you take a limit as $n \to \infty$, you should not have $n$ in your expression anymore
	- RIP partial credit
		- This is for your own good
	- Know the difference between the asymptotic distribution and the approximate distribution for a large $n$
	- $\sqrt n (\bar Y - \mu) \overset{d}{\to} \mathcal{N}(0, \sigma^2)$ as $n \to \infty$
		- Note that there is no $n$ in the RHS
	- $\bar Y \dot\sim \mathcal{N}(\mu, \sigma^2 / n)$ given some large $n$

## Regression
- Basic setting: $Y = \beta_0 + \beta_1 X + \epsilon$
	- Let $\epsilon \perp X$, where both are r.v.s
- We often want to consider the conditional distribution $Y \mid X$
	- Or $\mathbb{E}[Y \mid X]$
	- Quantiles are also well defined
- Logistic regression: special case where $Y = \{0, 1\}$ (i.e., data is binary)
- Poisson regression: $Y$ is count data (integers)

### Gaussian linear regression
- For now, assume $Y$ is Normal (Gaussian, in some courses/contexts)

$$
\begin{aligned}
Y | X &\sim \mathcal{N}(\theta_0 + \theta_1 X, \sigma^2) \\
\epsilon &= Y - \theta_0 - \theta_1 X \\
\mathbb{E}[Y|X] &= \theta_0 + \theta_1 X
\end{aligned}
$$

- $\epsilon$ is called **error** (unobservable)
	- Not observable w/o info about $\theta_0, \theta_1$
	- For plotting, we use the **residual** (observable)
- Residual: $\hat\epsilon = Y - \hat\theta_0 - \hat\theta_1 X$
	- $\hat \theta_0 + \hat \theta_1 X$ is the fitted/predicted value
- Still need to show independence of $X$ and $\epsilon$
	- $\epsilon | X \sim \mathcal{N} (0, \sigma^2)$
	- Conditional distribution does not depend on $X$
- $X$ does not have to be Normal for the assumptions of this model
- For rest of problem, assume $Y = \theta X + \epsilon$
- Estimators
	- Assuming no intercept and mean of 0 for both $X, Y$

$$
\hat\theta = \frac{
\sum_{j = i}^n X_j Y_j
}{
\sum_{j = 1}^n X_j^2
}
$$

- We have

$$
\textrm{Cov}(Y, X) = \textrm{Cov}(\theta X, X) + \textrm{Cov}(\epsilon, X) \to \theta = \frac{\textrm{Cov}(X, Y)}{\textrm{Var}(X)}
$$

- So by standard definitions of covariance and variance, 

$$
\theta = \frac{
\sum_{j = i}^n (X_j - \bar X)(Y_j - \bar Y)
}{
\sum_{j = 1}^n (X_j - \bar X)^2
}
$$

### Logistic regression
- Bernoulli is unique because all binary data is Bernoulli
	- Unlike continuous distributions, which can take many distributions (which we then try to coerce to Normal)

$$
\mu(x) = \mathbb{E}[Y | X = x] = P(Y = 1 | X = x)
$$

- The form $\theta_0 + \theta_1 X$ is not generally applicable because the RHS is not necessarily bounded by $0, 1$
- So what functions can we use to constrain this?
	- Relevant choice is a CDF, e.g., $F(\theta_0 + \theta_1 X)$ where $F$ is some valid CDF
	- Most common choice is logistic distribution (logit regression)
	- We can also use $\Phi$ (probit regression)
	- Results usually similar between logit and probit
		- Probit not in closed form
		- Logit function is natural parameter of NEF of binomial

$$
\textrm{logit}(p) = \log \left( \frac{p}{1 - p} \right), \textrm{logit}^{-1}(p) = \frac{e^x}{1 + e^x}
$$

- The inverse logit is also the **sigmoid** function
	- This is also the logistic CDF

$$
P(Y = 1 | X = x) = \frac{e^{\theta x}}{1 + e^{\theta x}}
$$

- Likelihood function given as

$$
L(\theta) = 
\prod_{j = 1}^n
\left(
\frac{e^{\theta x_j}}{1 + e^{\theta x_j}}
\right)^{y_j}
\left(
\frac{1}{1 + e^{\theta x_j}}
\right)^{1 - y_j}
$$

- No closed-form solution for the MLE