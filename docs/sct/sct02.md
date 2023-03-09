---
layout: post
title: Section 02 - Asymptotics and MLE properties
date: 2023-02-09
section: 2
---
## Content
- [Asymptotics](#asymptotics)
- [Consistency](#consistency)
- [Score function and Fisher information](#fisher)
- [Kullback-Leibler divergence](#kl)
- [Cramer-Rao lower bound](#crlb)
- [Properties of MLE](#mle)
- [Problems](#qs)

## Asymptotics {#asymptotics}
**Thm** (Slutsky's Theorem). If $X_1, X_2, \dots$ and $Y_1, Y_2, \dots$ are sequences of r.v.s such that $X_n$ converges to $X$ in distribution and $Y_n$ converges to $c$ in probability, then

1. $X_n + Y_n \overset{d}{\to} X + c$
2. $X_n Y_n \overset{d}{\to} cX$
2. $X_n / Y_n \overset{d}{\to} X / c$ for $c \neq 0$

- What if the sequences both converge in distribution?
- What if both sequences converge in probability?

**Thm**. (Continuous mapping theorem). If $X_1, X_2, \dots$ is a sequence of r.v.s and $g$ is a continuous function, then 

1. If $X_n \overset{d}{\to} X$, then $g(X_n) \overset{d}{\to} g(X)$
2. If $X_n \overset{p}{\to} X$, then $g(X_n) \overset{d}{\to} g(X)$

- Which of the above is redundant?

**Delta method**. Relates to convergence in distribution. Let $g$ be differentiable such that $g'(\mu) \neq 0$. 

$$
\begin{aligned}
\frac{\sqrt n (Y_n - \mu)}{\sigma} &\overset{d}{\to} \mathcal{N}(0, 1) \\
\frac{\sqrt n (g(Y_n) - g(\mu))}{\mid g'(\mu)\mid \sigma} &\overset{d}{\to} \mathcal{N}(0, 1) 
\end{aligned}
$$

**Check**: Why do we use absolute value in the denominator?

## Consistency {#consistency}
**Def** (Consistency). An estimator $\hat\theta$ is **consistent** for the estimand $\theta^\ast$ if $\hat\theta \overset{p}{\to} \theta^\ast$ as $n \to \infty$

- How to prove consistency?
	- $\textrm{MSE}(\hat\theta, \theta) \to 0$
	- $E(X_i) = \mu, \bar{X} \overset{p}{\to} \mu$
	- CMT
	- Definition of convergence $P(\mid \hat\theta - \theta\mid  > \epsilon) \to 0, \forall \epsilon$
- What would proof by CMT look like? When would we apply it?

## Score function and Fisher information {#fisher}
### Well-specified
- True data generating distribution: $\vec{Y} \sim G_{\vec Y}(\vec y)$
- Infer with model $\vec{Y} \sim F_{\vec{Y}} (\vec{y}\mid \theta)$
- If there exists $\theta^\ast$ such that $G_{\vec{Y}} = F_{\vec{Y}}(\vec{y} \mid  \theta^*)$ then the model is **well-specified** and $\theta^\ast$ is called the true value

### Score function
- For continuously differentiable log-likelihood, the score function is defined as

$$
s(\theta; \vec{Y}) = \frac{\partial \ell (\theta; \vec{Y})}{\partial \theta}
$$

- If the model is correctly specified and regularity conditions hold

$$
\begin{aligned}
E(s(\theta^*; \vec{Y})) &= 0 \\
\textrm{Var}(s(\theta^*; \vec{Y})) = -E(s'(\theta^*; \vec{Y}))
\end{aligned}
$$

- Take expectation w.r.t. the true distribution
- Regularity conditions
	- Support of $\vec{Y}$ does not depend on $\theta$ (when would this happen?)
	- Expectations and derivatives exist
- $\theta^\ast$ maximizes the expectation of log-likelihood
- Equation 2 also called information equality

### Fisher information
**Def** (Fisher information). Fisher information is defined as 

$$
\mathcal{I}_{\vec{Y}}(\theta^*) = \textrm{Var}(s(\theta^*; \vec{Y}))
$$

- Often abbreviated $\mathcal{I}(\theta)$
- If model is well-specified, Fisher information is

$$
\mathcal{I}(\theta) = -E(s'(\theta^*; \vec{Y})) = E(s(\theta^*; \vec{Y})^2)
$$

- Fisher info for parameter transformation
	- Let parameter $\tau$ be $\tau = g(\theta)$
	- Let $g$ be injective and differentiable

$$
\mathcal{I}(\tau) = \frac{\mathcal{I}(\theta)}{g'(\theta)^2}
$$

## Kullback-Leibler divergence {#kl}
**Def**. (Kullback-Leibler divergence). Kullback-Leibler divergence, also called K-L divergence or relative entropy, is defined as

$$
D(F\mid \mid G) = 
E_f \left[
\log \frac{f(\vec{X})}{g(\vec{X})}
\right] = 
\int_{-\infty}^\infty
\log \frac{f(\vec{x})}{g(\vec{x})} f(\vec{x}) d(\vec{x})
$$

- $f, g$ are PDFs for distributions $F, G$
- $D(F\mid \mid G) \geq 0$ by Jensen's inequality
- For two distributions in the same family w/ different parameters $\theta_1, \theta_2$, we can write $D(F_{\theta_1} \mid \mid  F_{\theta_2}) = D(\theta_1 \mid \mid  \theta_2)$
- Suppose true data generating distribution $\vec{X} \sim F_{\vec{X}} (\vec{x} \mid  \theta^*)$ and correctly specified model $F_{\vec{X}}(\vec{x}\mid  \theta)$
	- Under regularity conditions, $\theta^\ast$ minimizes $D(\theta^*\mid \mid \theta)$
- Curvature (second derivative) at $\theta^\ast$ of K-L divergence is the Fisher info

## Cramer-Rao lower bound {#crlb}
**Thm** (Cramer-Rao Lower Bound, CRLB). Under regularity, if $\hat\theta$ is unbiased for $\theta$, then

$$
\textrm{Var}(\hat\theta) \geq \frac{1}{n \mathcal{I}_1(\theta^*)}
$$

- $\mathcal{I}_1$ is Fisher info for one observation
- Lower bound on MSE for **unbiased estimators**
- MLE achieves CRLB asymptotically
	- Not unique
- The CRLB for estimators (no assumptions about bias), we have

$$
\textrm{Var}(\hat\theta) \geq \frac{g'(\theta^*)^2}{n \mathcal{I}_n (\theta^*)}
$$

- $g(\theta) = E(\hat\theta)$

## Properties of MLE {#mle}
- The MLE is consistent
	- With regularity conditions and correctly specified model
- Asymptotic distribution of MLE
	- Regularity
	- Correctly specified model

$$
\sqrt n (\hat\theta - \theta^*) \overset{d}{\to} \mathcal{N}\left(0, \frac{1}{\mathcal{I}(\theta^*)}\right)
$$

- The variance of MLE is approximately $\dfrac{1}{n \mathcal{I}_1(\theta^*)}$
	- Why does this make sense?

# Problems {#qs}
## 1 Typo searching

Let $Y_j$ be the number of typos on page $j$ of a certain book, and suppose that the $Y_j$ are i.i.d. $Pois(\lambda)$, with $\lambda$ unknown. Let

$$\theta = P(Y_j \geq 1 \mid  \lambda),$$

the probability of a page having at least one typo. The first $n$ pages of the book are proofread extremely carefully, so $Y_1,\dots,Y_n$ are observed. 

### 1.1 MLE

Find the MLE $\hat{\lambda}$ of $\lambda$.

### 1.2 Approximate distribution

Show that $\hat{\lambda}$ is approximately Normal for $n$ large, and give the parameters. 

### 1.3 MLE

Find the MLE $\hat{\theta}$ of $\theta$. 

### 1.4 Distribution of $\hat\theta$

Find the distribution of $\hat\theta$ in three different ways: 

#### 1.4.1 Fisher information 

Use Fisher information and the result about the asymptotic distribution of the MLE.
 
#### 1.4.2 Delta method
 
#### 1.4.3 Simulation

Use simulation for the case where $n=10$ and the true value is $\lambda^* = 1$, performing at least $10^4$ replications.
 
#### 1.4.4 Compare

Compare the results of the three approaches above: how similar or different are they? If they differ substantially, which do you trust the most? 

#### 1.4.5 Discuss

Discuss the relative advantages and disadvantages of the three approaches above (e.g., in terms of accuracy, generality, and ease of computation). 

## 2 Score and Fisher info of Uniform

Suppose we have $X_1,\cdots, X_n \overset{i.i.d.}{\to} \textrm{Unif}(0,\theta), \theta >0$.

### 2.1 Finding the score

Let $n=1$, write down score function $s(\theta; x)$. Find $E[s(\theta^\ast; X_1)]$, where $\theta^\ast$ is the true parameter.

### 2.2 Checking properties of score

With $n=1$, find $\mathbb{E}\left[(s(\theta^\ast; X_1))^2\right]$, $\textrm{Var}(s(\theta^\ast; X_1))$, and $-\mathbb{E}\left[\frac{\partial s(\theta^\ast; X_1)}{\partial \theta^\ast}\right]$. Are they equal? Can you explain this?

### 2.3 Fisher w/o regularity

In fact, when the regularity conditions does not hold, we re-define Fisher information as $\mathcal{I}_{\mathbf{X}}(\theta^\ast)=E[(s(\theta^\ast; \mathbf{X}))^2]$. Find  $\mathcal{I}_{X_1}(\theta^\ast)$ in this setting.

### 2.3 More Fisher

From this part, consider a general $n>0$. Find $\mathcal{I}_{\mathbf{X}}(\theta^\ast)$.

### 2.4 MLE

Let $\hat{\theta}$ be the MLE for $\theta$. For a constant $\epsilon>0$, find $\Pr(\theta-\hat{\theta}>\epsilon)$ and use this to prove the consistency of $\hat{\theta}$.

### 2.5 Bias

Show that the MLE is biased ("story proof" is fine). Propose an unbiased estimator of $\theta$ in the form of $c\hat{\theta}$, where $c$ is constant. What is the variance of this estimator?

### 2.6 Variance of (2.5)

How does the variance of your estimator in 2.5 compare to the inverse of Fisher information?