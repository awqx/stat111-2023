---
layout: post
title: Lecture 06 - Delta method, MLE properties
date: 2023-02-09
lecture: 6
---

## Review from yesterday's concepts
- See [Lecture 05](lct05-asymptotics) for more details
- Let $Y_i$ be i.i.d., with finite mean $\mu$ and variance $\sigma^2$
- **Law of large numbers**: $\bar Y_n \overset{p}{\to} \mu$
- **Central limit theorem**: $\sqrt{n}(\bar Y_n - \mu) \overset{d}{\to} \mathcal{N}(0, \sigma^2)$
- **Continuous mapping theorem**: Let $g$ be a continuous function. If $X_n \to X$, then $g(X_n) \to g(X)$
- Convergence in probability is much stronger than convergence in distribution
	- Unless the convergence is to a constant
- **Slutsky's theorem**: If $X_n \overset{d}{\to} X$ and $Y_n \overset{d}{\to} c$ where $c$ is a constant, then we can "do arithmetic" with functions of these r.v.s in the limit
	- $X_n + Y_n \to X + c$, for example, with similar results for subtraction, multiplication, and divisions

### Examples
**Ex**. Let $Y_1, Y_2, \dots$ be i.i.d. with mean $\mu \neq 0$ and variance $\sigma^2$. 

Create a statistic

$$
T_h = 
\frac{\sqrt{n} (\bar Y_n - \mu)}{Y_n^3 + \mu^3}
$$

We can solve the components as

$$
\begin{aligned}
\sqrt{n}(\bar Y_n - \mu) \overset{d}{\to} \sigma Z, Z \sim \mathcal{N}(0, 1) \textrm{ by CLT} \\
\bar{Y}_n \to \mu \textrm{ by LLN } \\
\bar Y_n^3 + \mu^3 \to 2 \mu^3 \textrm{ by CMT} \\
T_h \overset{d}{\to} \frac{\sigma Z}{2 \mu^3} \sim \mathcal{N}(0, \sigma^2 / (4\mu^6))
\end{aligned}
$$


## Delta method
- Last tool for asymptotics
- Name is not intuitive
	- It's a theorem, not a method

**Thm** (Delta method). Let $g$ be a differentiable function. Suppose $\sqrt{n} (T_n - \theta) \overset{d}{\to} \mathcal{N}(0, \sigma^2)$, where $T_n$ is an r.v. and $\theta$ is a constant. Then, 

$$
\sqrt{n} (g(T_n) - g(\theta)) \overset{d}{\to}
\mathcal{N} 
\left(
0, (g'(\theta))^2 \sigma^2
\right)
$$

- Joe thinks this looks wrong
	- How is it still Normal?
	- How did the mean stay zero? Jensen's inequality would imply otherwise
		- $\mathbb{E}[T_n] \approx \theta$ but does $\mathbb{E}[g(\theta)] \approx g(\theta)$? If $g$ is convex or concave, we can predict the direction of inequality of $\mathbb{E}[g(\theta)], g(\mathbb{E}[\theta])$
- Joe's proposed other names
	- "Indestructibility of the Normal"
- Heuristic proof
	- Note that $T_n - \theta \to 0$ because if not, the original convergence to Normal would not work (because $\sqrt{n} \to \infty$). 
		- So this convergence in probability will help with keeping the mean zero
	- We can do a linear approximation around $\theta$
		- $g(t) \approx g(\theta) + g'(\theta)(t - \theta)$
		- Replace number with random variable
		- $g(T_n) \approx g(\theta) + g'(\theta)(T_n - \theta)$
	- Do some algebra so it resembles the LHS
		- $\sqrt{n} (g(T_n) - g(\theta)) \approx g'(\theta) \sqrt{n} (T_n - \theta)$
		- $g'(\theta)$ is a constant, $g'(\theta) \sqrt{n} (T_n - \theta)$ converges to a distribution $\sigma Z: Z \sim \mathcal{N}(0, 1)$
	- We can use Slutsky to have convergence of the RHS
	- We get $\sqrt{n} (g(T_n) - g(\theta)) \approx g'(\theta) \sigma Z$, which is the expression we want
- We also want to show $T_n \overset{p}{\to} \theta$ (consistency)

$$
T_n - \theta = \frac{1}{\sqrt n} \sqrt n (T_n - \theta)
$$

- We have that as $n \to \infty, \frac{1}{\sqrt{n}} \to 0$ and $\sqrt n (T_n - \theta) \to \mathcal{N}(0, 1)$. By Slutsky, consistency follows
	- Note we're using sameness of convergence for convergence to a constant

**Thm** (Delta method restatement). If $T_n \dot\sim \mathcal{N}(\theta, \frac{\sigma^2}{n})$, then $g(T_n) \dot\sim \mathcal{N}\left(g(\theta), \frac{g'(\theta)^2 \sigma^2}{n}\right)$ 

### Examples
**Ex** Let $T \sim \textrm{Pois}(\lambda)$. The Poisson distribution is $\approx \mathcal{N}(\lambda, \lambda)$ for large $\lambda$, where $\lambda \geq 0$ and preferable $\geq 100$. 

We have $\sqrt{T} \sim \mathcal{N}(\sqrt{\lambda}, 1/4)$. When $g(x) = x^{1/2}$, we have $g'(x) = \frac{1}{2} x^{-1/2}$. This has the effect of stabilizing the variance. No matter how large $\lambda$ is, this particular transformation will always result in a variance of $1/4$. 

## Properties of MLE
Under regularity conditions, $\hat\theta$ exists and is consistent (converges to the estimand in probability, i.e., $\hat\theta \overset{p}{\to} \theta^\ast$. 

Note on notation: $\theta^\ast$ is the *true* estimand. Sometimes, $\theta$ (no asterisk) means "any" $\theta$ we could consider (like the statement of the Delta method). Sometimes it's hard to be consistent, so ask if you're confused. 

- $\hat\theta$ is asymptotically Normal
- $\hat\theta$ is asymptotically unbiased
- Asymptotic variance is best possible

**Thm** (Asymptotic distribution of the MLE). The distribution of the MLE converges to 

$$
\sqrt{n}(\hat\theta_n - \theta^\ast) 
\overset{d}{\to}
\mathcal{N}(0, ???)
$$

- $???$ is a secret that will be revealed later
- Joe is looking for good names for this theorem
	- Post on Ed if you think of anything

### Score and Fisher info

**Def** (Score). The score function is the derivative of the log-likelihood. If $L(\theta; y)$ is the likelihood and $\ell(\theta; y)$ is $\log L(\theta; y)$, we have that

$$
s(\theta; y) = \frac{\partial}{\partial \theta} \ell(\theta; y), 
$$

(assuming that $\ell(\theta; y)$ is single-dimensional). 

**Def** (Fisher information). The fisher information is

$$
\mathcal{I}_Y(\theta) = 
\textrm{Var}(s(\theta; Y); \theta),
$$

where $Y$ is random and the variance is computed under the assumption that the true data-generating estimand is $\theta$. 

**NO BAYES STUFF THIS WEEK**. 

**Thm** (Score expectation). The expected value of the score is 0. 

*Proof*: We can use differentiation under the integral sign (DUThIS) (also called Leibniz's rule). 

$$
\frac{d}{d\theta} \int_{-\infty}^{\infty} h(x, \theta) dx = 
\int_{-\infty}^{\infty} \frac{d}{d\theta} h(x, \theta) dx.
$$

Applying this to expectation, we have

$$
\begin{aligned}
\mathbb{E}[s(\theta; Y)] &= 
\int_{-\infty}^{\infty}
s(\theta; y)
f(y; \theta) dy \\
&= 
\int_{-\infty}^{\infty} 
\frac{L'(\theta; y)}{L(\theta; y)} f(y; \theta) dy \\
&= 
\int_{-\infty}^{\infty}
\frac{\partial}{\partial \theta} f(y; \theta) dy \\
&= 
\frac{\partial}{\partial \theta}
\int_{-\infty}^{\infty}
f(y; \theta) dy \\
&= 
\frac{\partial}{\partial \theta} 1 \\
&= 0.
\end{aligned}
$$

**Thm** (Score-info). $\mathbb{E}[s'(\theta; Y)] = -\mathcal{I}_Y(\theta)$