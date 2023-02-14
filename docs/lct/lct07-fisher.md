---
layout: post
title: Lecture 07 - MLE properties
date: 2023-02-14
lecture: 7
---

## MLE asymptotics
Under "regularity conditions", we have that

$$
\sqrt{n} (\hat\theta_n - \theta^\ast) \overset{d}{\to}
\mathcal{N}
(0, I^{-1}_1 (\theta^\ast)), 
\quad 
\hat\theta_n \overset{p}{\to} \theta^\ast
$$

- Joe recommends the movie "Quantum Hoops"
	- Documentary about CalTech basketball team
	- Lost 200 games in a row without winning
	- How do we estimate $\Pr(\textrm{Win})$?

<img src="{{site.url}}/img/quantum_hoops.jpg" style="display: block; margin: auto;" />

### Regularity conditions
- Data is i.i.d. with density $f_\theta(y)$
- Support of the density does not depend on $\theta$ (the estimand of interest)
	- Counterexample: estimating $\theta$ for $\textrm{Unif}(0, \theta)$
- $\frac{\partial^3}{\partial \theta^2} f(y; \theta)$ exists
- $\theta^\ast$ is *not* on the boundary of parameter space
- Can DUThIS
	- Properties of expectation for score function requires DUThIS

### Score function properties

$$
\mathbb{E} s(\theta; Y) = 0, \quad
-\mathbb{E} s'(\theta; Y) = \mathbb{E} s^2 (\theta; Y)
$$

- High variance in the score function will be helpful
	- Intuitively, this seems wrong because variance seems "noisy"
	- But in this case, variance means that observations are especially informative
- The asymptotic variance of the MLE is the *inverse* of Fisher Info
	- So high variance in score function will lead to better confidence intervals for the MLE

**Thm** (Information equality). We have that $-\mathbb{E}[\ell''(\theta; Y)] = \mathbb{E}[(\ell')^2(\theta; Y)] = \mathcal{I}_Y(\theta)$. Whichever is easiest depends on the problem. 

**Ex** (Geometric distribution). Let $Y_1, \dots, Y_n \overset{i.i.d.}{\sim} \textrm{Geom}(p)$. Show that $\mathcal{I}_{\mathbf{Y}}(r) = n \mathcal{I}_1 (p)$. 

$$
\begin{aligned}
\ell(\theta; \mathbf{Y}) &= 
\sum_{i = 1}^n \ell(\theta; Y_i) \\
s(\theta; \mathbf{Y}) &= 
\sum_{i = 1}^n s(\theta; Y_i) \\
\textrm{Var}
(s(\theta; \mathbf{Y})) &= 
n \textrm{Var} (s (\theta; Y_1)).
\end{aligned}
$$

We can solve for the Fisher info in two different ways. I'll skip the derivation of score function, but it should be simple based on the Geometric PMF. 

The score function is given as $s_1(p) = \frac{1}{p} - \frac{Y}{1-p}$. 

$$
\textrm{Var}(s_1(p)) = \textrm{Var}\left(\frac{Y}{1 - p}\right) = \frac{1}{qp^2}: q = 1 - p.
$$

And, using the LHS of the information inequality: 

$$
\begin{aligned}
s'(\theta; Y) &= -\frac{1}{p^2} -\frac{Y}{(1 - p)^2} \\
\mathbb{E}[s'(\theta; Y)] &= -\frac{1}{p^2} - \frac{q}{p} \frac{1}{q}
= -\left(
\frac{1}{p^2} - \frac{1}{p}\right) 
= -\frac{q}{p^2}
\end{aligned}
$$

**Thm** (Information inequality or Cramer-Rao Lower Bound, CRLB). Under regularity conditions, if $\hat\theta$ is unbiased, then 

$$
\textrm{Var}(\hat\theta) \geq \frac{1}{\mathcal{I}_{\vec{Y}} (\theta^\ast}
$$

where the above is also the MSE because the estimator is unbiased. 

Although the MLE may be biased, it is unbiased asymptotically. The asymptotic variance also achieves CRLB. However, the MLE is not unique in achieving the CRLB. 

*Proof*: Let $\hat\theta = T(Y), S = s(\theta, Y)$. Let $\hat\theta$ be unbiased. We have

$$
\begin{aligned}
\textrm{Cov}(S, T) &= 
\mathbb{E}(ST) - \mathbb{E}[S]\mathbb{E}[T] \\
&= 
\int \frac{\partial}{\partial \theta} 
\log f(y; \theta) T(y) f(y; \theta) dy \quad \textrm{by LOTUS} \\
&= 
\int \frac{\partial f(y; \theta)}{\partial \theta} T(y) dy 
\quad \textrm{by the chain rule}
\\
&= 
\frac{\partial}{\partial \theta}
\int f(y; \theta) T(y) dy 
\quad \textrm{by DUThIS} \\
&= \frac{\partial}{\partial \theta} \mathbb{E} \hat\theta 
\quad \textrm{by LOTUS}
\\ &= 1
\quad \textrm{by unbiasedness}.
\end{aligned}
$$

We then have $\textrm{Cov}^2(S, T) \leq \textrm{Var}(S) \textrm{Var}(T)$ by Cauchy-Schwarz and correlation being upper bounded by 1. We then have

$$
\begin{aligned}
\textrm{Var}(S) \textrm{Var}(T) &\geq 1 \\
\textrm{Var}(\hat\theta) &\geq \frac{1}{\textrm{Var}(S)} \\
&= \frac{1}{\mathcal{I}_{\vec{Y}}(\theta^\ast)}
\end{aligned}
$$


We can now also prove the Delta method. 

**Idea**: Taylor expand the score function about $\theta^\ast$. Note that the second line follows because $s(\hat\theta) = 0$. The third line follows from noting that the RHS is composed of sample means. We can then use CLT on the numerator, LLN in the denominator, and Slutsky's. 

$$
\begin{aligned}
s(\hat\theta) &\approx
s(\theta^*) + (\hat\theta - \theta^\ast) s'(\theta^*) \\
\sqrt{n}(\hat\theta - \theta^\ast) &\approx \sqrt{n} \frac{s(\theta^\ast) / n}{-s'(\theta^\ast) / n} \\
&\overset{d}{\to}
\frac{\sqrt{\mathcal{I}_1(\theta^\ast)}Z}{\mathcal{I}_1(\theta^\ast)} \\
&\sim \mathcal{N}\left(0, \frac{1}{\mathcal{I}_1 (\theta^\ast)}\right).
\end{aligned}
$$