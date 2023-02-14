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