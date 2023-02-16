---
layout: post
title: Section 02 (Solutions)
date: 2023-02-09
section: 2.1
---
## Content
- [Typo searching](#typos)

## 1 Typo searching {#typos}

Let $Y_j$ be the number of typos on page $j$ of a certain book, and suppose that the $Y_j$ are i.i.d. $Pois(\lambda)$, with $\lambda$ unknown. Let

$$\theta = P(Y_j \geq 1 \mid  \lambda),$$

the probability of a page having at least one typo. The first $n$ pages of the book are proofread extremely carefully, so $Y_1,\dots,Y_n$ are observed. 

### 1.1 MLE

> Find the MLE $\hat{\lambda}$ of $\lambda$.

Let $\vec{Y}$ be $(Y_1, \dots, Y_n)$. By the PDF of the Poisson, we have the likelihood and log-likelihood function of $\lambda$ as 

$$
L(\lambda; \vec{Y}) = \prod_{j = 1}^n \frac{e^{-\lambda}\lambda^{Y_j}}{Y_j!} = e^{-n\lambda}\lambda^{\sum_{j = 1}^nY_j}
$$

which gives us a log-likelihood of 

$$
\ell(\lambda; \vec{Y}) = -n\lambda + \ln\lambda\sum_{j = 1}^nY_j.
$$

Taking the derivative of the log-likelihood and setting it to zero, we can obtain the MLE as

$$
\frac{d \ell(\lambda; \vec{Y})}{d\lambda} = -n + \frac{\sum_{j = 1}^nY_j}{\lambda} = 0
$$

and solving the above gives the MLE

$$
\hat\lambda = \frac{\sum_{j = 1}^nY_j}{n}.
$$

The second derivative of the log-likelihood is negative for all $\lambda$ (it is of the form $-\lambda^{-2}$ times a constant), so we can confirm this is the maximum. 

### 1.2 Approximate distribution

> Show that $\hat{\lambda}$ is approximately Normal for $n$ large, and give the parameters. 

The MLE $\hat\lambda$ satisfies the regularity conditions, is based on a scalar parameter, and is based on i.i.d. observations. We can therefore apply the standard asymptotics for the MLE. 

To determine the Fisher information, we can use the derivative of the log-likelihood found in part (a). We have $s(\lambda; \vec{Y}) = -n + \dfrac{\sum_{j = 1}^nY_j }{\lambda}$. Taking the variance, we have

$$
\textrm{Var} (s(\lambda; \vec{Y})) = \frac{1}{\lambda^2}\textrm{Var} \left(-\sum_{j = 1}^nY_j\right) = \frac{n}{\lambda^2}\textrm{Var} \left(\sum_{j = 1}^nY_j \right) = \frac{n}{\lambda},
$$

where the last equality follows from $Y_j$ being i.i.d. We have $\mathcal{I}_{\vec{Y}}(\lambda) = n\lambda$. We can then write our asymptotic distribution as 

$$
\hat\lambda\; \dot\sim \mathcal{N}\left(\lambda^*, \frac{\lambda^*}{n}\right).
$$

### 1.3 MLE

> Find the MLE $\hat{\theta}$ of $\theta$. 

We can use invariance of the MLE. By the Poisson distribution, $\theta$ is the complement of the probability $P(Y_j = 0)$, or $\theta = 1 - P(Y_j = 0 \mid  \lambda) = 1 - e^{-\lambda}.$

The MLE $\hat\theta$ is 

$$
\hat\theta = 1 - e^{-\hat\lambda} = 1 - e^{-\frac{\sum_{j = 1}^nY_j}{n}}.
$$

### 1.4 Distribution of $\hat\theta$

Find the distribution of $\hat\theta$ in three different ways: 

#### 1.4.1 Fisher information 

> Use Fisher information and the result about the asymptotic distribution of the MLE.

we can obtain the Fisher information by relating $\theta$ to $\lambda$ with a function. We have $g(\lambda) = 1 - e^{-\lambda}$, $g'(\lambda) = \lambda e^{-\lambda}$, and $\mathcal{I}_{\vec{Y}}(\lambda) = n\lambda$, so we can write

$$
\mathcal{I}_{\vec{Y}}(\theta) = \frac{\mathcal{I}_{\vec{Y}}(\lambda)}{(g'(\lambda))^2} = \frac{n\lambda}{\lambda^2e^{-2\lambda}} = \frac{n}{\lambda e^{-2\lambda}}
$$
 
Our asymptotic distribution is 

$$
\hat\theta\; \dot\sim\; \mathcal{N}\left(\theta, \frac{\lambda e^{-2\lambda}}{n}\right)
$$

We have that $\theta = 1 - e^{-\lambda} \to \lambda = -\log(1 - \theta)$ so we can make the substitution

$$
\hat\theta\; \dot\sim\; \mathcal{N}\left(\theta, \frac{-\log(1 - \theta)(1 - \theta)^2}{n}\right)
$$

#### 1.4.2 Delta method

By the delta method, we have that 

$$
g(\hat\lambda)\; \dot\sim\; \mathcal{N}\left(g(\lambda),\left(\frac{\partial g(\lambda)}{\partial \lambda}\right)^2\frac{\omega^2}{n}\right)
$$

where we have 

$$
\frac{\partial g(\lambda)}{\partial \lambda} = \lambda e^{-\lambda}, \omega^2 = \frac{1}{n\lambda},
$$

giving us the asymptotic distribution

$$
\hat\theta\; \dot\sim\; \mathcal{N}\left(\theta, \frac{\lambda e^{-2\lambda}}{n}\right) = \mathcal{N} \left(\theta, \frac{-\log(1 - \theta)(1 - \theta)^2}{n}\right).
$$

#### 1.4.3 Simulation

> Use simulation for the case where $n=10$ and the true value is $\lambda^* = 1$, performing at least $10^4$ replications.

```
set.seed(2020)
n <- 10
# x is a vector of Y_j
theta_mle <- function(x) {
  1 - exp(-sum(x)/n)
}

pois_trials <- replicate(100000, rpois(10, 1)) %>%
  data.frame()
  
# theta, averaged over all trials
mean(sapply(pois_trials, theta_mle))

# variance over all trials
var(sapply(pois_trials, theta_mle))
```

By the central limit theorem, we have that the distribution of $\hat\theta$ is approximately Normal for large enough $n$, so we  can write $\hat\theta\; \dot\sim\; \mathcal{N}(0.614, 0.0142)$.

#### 1.4.4 Compare

> Compare the results of the three approaches above: how similar or different are they? If they differ substantially, which do you trust the most? 

The approaches yield largely similar results. The distributions in (d) and (e) are identical. Both undergo around the same levels of approximation between conversion between the Fisher information and central limit theorem. 

Additionally, the mean and variance suggested by (d) and (e) is similar to the mean variance found in (f). When calculating based on (d) and (e), we have $\theta = 1 - e^{-1} \approx 0.632$ and $\textrm{Var} (\theta) = e^{-2}/10 \approx 0.0135$, which is consistent with the mean and variance ($0.614, 0.140$) obtained in the simulation. 

#### 1.4.5 Discuss

> Discuss the relative advantages and disadvantages of the three approaches above (e.g., in terms of accuracy, generality, and ease of computation). 

The simulation yields an accurate estimate for $\theta$ with enough repetitions, but may become computationally overwhelming when running multiple trials with very large $n$. In the case that $n$ is large, it may be easier to examine the Fisher-MLE and delta method. Conversely, with very small $n$, we would expect the Fisher-MLE and delta method to yield poor predictions as they both undergo a few layers of approximations that assume large sample sizes. Both the Fisher-MLE and delta method are more generally descriptive of the distribution. The Fisher-MLE and delta method can be derived with largely the same steps (Taylor, CLT), though the Fisher strategy does use LLN. In other situations, this may lead to another layer of approximation that raises disadvantages for the accuracy.  

## Score and Fisher info of Uniform

Suppose we have $X_1,\cdots, X_n \overset{i.i.d.}{\to} \textrm{Unif}(0,\theta), \theta >0$.

### 2.1 Finding the score

> Let $n=1$, write down score function $s(\theta; x)$. Find $E[s(\theta^\ast; X_1)]$, where $\theta^\ast$ is the true parameter.


$$
\begin{aligned}

L(\theta;x_1)&=f(x_1 \mid \theta) = \frac{1}{\theta} \mathbb{1}_{0\leq x_1\leq \theta} \\

\ell(\theta; x_1) &= \log L(\theta; x) = \log (\theta^{-1}) \mathbb{1}_{\theta\geq x_1}= -\log (\theta) \mathbb{1}_{\theta\geq x_1} \\

s(\theta;X_1) &=\frac{\partial \ell(\theta; X_1)}{\partial\theta} = \begin{cases}-\frac{1}{\theta}&  \theta >X_1\\ 0 & \theta <X_1\\ \end{cases} = -\frac{1}{\theta}, \ \text{since $\theta >X_1$ always true.}\\

\end{aligned}
$$

Why the indicator? Because based on the PDF of the Uniform, we would not be able to observe any values past our boundary. So the multiplicative indicator penalizes any proposed $\theta$ values that would be inconsistent with our observations.

So taking expectation with respect to $X_1\sim \textrm{Unif}(0,\theta^\ast)$:

$$
\begin{aligned}
E[s(\theta^\ast;X_1)]&=\int_{0}^{\theta^\ast}\left(-\frac{1}{\theta^\ast}\right)\cdot f_{X_1}(x_1 \mid \theta^\ast)dx_1\\
&=\int_{0}^{\theta^\ast}\left(-\frac{1}{\theta^\ast}\right)\cdot\frac{1}{\theta^*}dx_1 = -\frac{1}{\theta^*} \neq 0 .
\end{aligned}
$$

### 2.2 Checking properties of score

> With $n=1$, find $\mathbb{E}\left[(s(\theta^\ast; X_1))^2\right]$, $\textrm{Var}(s(\theta^\ast; X_1))$, and $-\mathbb{E}\left[\frac{\partial s(\theta^\ast; X_1)}{\partial \theta^\ast}\right]$. Are they equal? Can you explain this?

$$
E[(s(\theta^\ast;X_1) )^2]=\int_{0}^{\theta^\ast}(-\frac{1}{\theta^\ast})^2\cdot \frac{1}{\theta^\ast}dx_1=\frac{1}{(\theta^\ast)^2}
$$

and because $s(\theta;X_1)=-\frac{1}{\theta}$ is constant, 

$$
\textrm{Var}(s(\theta;X_1))=0.
$$

We also have

$$
\ell''(\theta; X_1) = 
\frac{\partial^2}{\partial \theta^2}\ell'(\theta; X_1) 
= \begin{cases}\frac{1}{\theta^2}&  \theta >X_1 \\ 
0 & \theta <X_1 \\ \end{cases} = \frac{1}{\theta^2}: \theta > X_1
$$

Taking expectation with respect to $X_1\sim \textrm{Unif}(0,\theta^\ast)$:

$$
-\mathbb{E}\left[\frac{\partial s(\theta^\ast; X_1)}{\partial \theta^\ast}\right] = - E[\ell''(\theta^\ast; X_1)] = -\frac{1}{(\theta^*)^2}
$$

None of our equalities hold when regularity conditions do not hold. Specifically, because the support of $X$ depends on $\theta$ and the MLE occurs at a "boundary", we have off effects on the Fisher info. 

### 2.3 Fisher w/o regularity

> In fact, when the regularity conditions does not hold, we re-define Fisher information as $\mathcal{I}_{\mathbf{X}}(\theta^\ast)=E[(s(\theta^\ast; \mathbf{X}))^2]$. Find  $\mathcal{I}_{X_1}(\theta^\ast)$ in this setting.

The new Fisher information is 

$$
\mathcal{I}_{X_1}(\theta^\ast)=E[(s(\theta;X_1) )^2]=\frac{1}{(\theta^\ast)^2}.
$$

### 2.3 More Fisher

> From this part, consider a general $n>0$. Find $\mathcal{I}_{\mathbf{X}}(\theta^\ast)$.


For given observed data $x_1,\cdots, x_n$, its joint density is given by 
$$
f_{\mathbf{X}}(\mathbf{x}|\theta) = \prod_{i=1}^{n} \frac{1}{\theta}\mathbb{1}_{0\leq x_i\leq \theta} = \theta^{-n} \prod_{i=1}^{n}\mathbb{1}_{0 \leq x_i \leq \theta}
$$

From the joint density, we can see that the joint density is non-zero at $\theta \geq \text{max}\{\mathbf{x}\}$.

Log-likelihood:

$$
\ell(\theta; \mathbf{x}) = -n\log(\theta)\cdot \mathbb{1}_{\theta \geq \text{max}\{\mathbf{x}\}} 
$$

The score:

$$
s(\theta; \mathbf{X})=  \begin{cases}\frac{-n}{\theta}&  \theta >\text{max}\{\mathbf{X}\}\\ 0 &\theta <\text{max}\{\mathbf{X}\}\\ \end{cases} =\frac{-n}{\theta} \\
$$

So the Fisher information:

$$
\mathcal{I}_{\mathbf{X}}(\theta^\ast) = E[(s(\theta^\ast; \mathbf{X}))^2]=\frac{n^2}{(\theta^\ast)^2}.
$$

Here, $\mathcal{I}_{\mathbf{X}} (\theta^\ast) \neq n  \mathcal{I}_{X_1}(\theta^\ast)$, unlike the case where regularity conditions hold.
 		
### 2.4 MLE

> Let $\hat{\theta}$ be the MLE for $\theta$. For a constant $\epsilon>0$, find $\Pr(\theta-\hat{\theta}>\epsilon)$ and use this to prove the consistency of $\hat{\theta}$.



### 2.5 Bias

> Show that the MLE is biased ("story proof" is fine). Propose an unbiased estimator of $\theta$ in the form of $c\hat{\theta}$, where $c$ is constant. What is the variance of this estimator?



### 2.6 Variance of (2.5)

> How does the variance of your estimator in 2.5 compare to the inverse of Fisher information?