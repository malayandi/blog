---
layout: post
title:  "Lower(-Bound) Your Expectation"
excerpt: "We take a closer look at Jensen's inequality and offer a proof of it, before looking at two applications of it: in deriving the AM-GM inequality and in proving the Rao-Blackwell theorem."
author: by Chin Chang and Andy Palan
date:   2018-08-01 00:00:00 -0800

categories: "probability"
---

***Prerequisites:*** *The following discussion of Jensen's Inequality assumes basic knowledge of probability theory (e.g., expectation, conditional probability).*

Jensen's inequality is a fairly well-known and widely-used theorem in probability theory. It is one of those results that would be mentioned in passing: I cannot begin to count the number of times I have heard the statement "This line follows, of course, by Jensen's inequality". Yet, we never thought about it too much; perhaps, this was because the result *seems* intuitive enough to simply accept.

Recently, we came across Jensen's inequality once again while thinking about the Expectation-Maximization algorithm. Tired of simply *accepting* this result, we decided to spend some time thinking about it and, having finally taken the time to do so, we were struck by how the result manages to be so powerful while being so simple, leading us to where we are now.

In this post, we will discuss Jensen's inequality and offer a proof of it. We will then discuss a couple key applications of Jensen's inequality: an alternate proof of the AM-GM inequality and the proof of the Rao-Blackwell theorem.

## Jensen's Inequality

Recall that the variance of random variable $X$ is defined as $Var(X) = \mathbb{E}X^2 - (\mathbb{E}X)^2$ and that $Var(X) \geq 0 \ \forall\ X$. If we let $f(x) = x^2$, then we can combine these statements to write

$$
\begin{align}
	Var&(X) = \mathbb{E}[f(X)] - f(\mathbb{E}X) \geq 0\\
	&\Rightarrow \mathbb{E}[f(X)] \geq \mathbb{E}[f(X)]
\end{align}
$$

It is then natural to ask, for what functions $f$ does this result hold? Jensen's inequality answers this question.

***Theorem:** (Jensen's Inequality) Let $X$ be a random variable and $f$ be a convex function. Then,
$$
\begin{align}
    \mathbb{E}[f(X)] \geq f(\mathbb{E}X).
\end{align}
$$*

This result may seem especially obvious when visualized in the simple setting below. Let $X\sim Unif[-1,1]$ and $f$ be some convex function on this interval. Then, we can visualize the result as follows:

![Figure 1]({{site.url}}/assets/images/jensens_post/plt.png)

Loosely speaking, the convex, "bowl-shaped" nature of $f$ ensures that the mean of $X$ is mapped to near the bottom of this "bowl", ensuring that the mean of $f(X)$ will lie above it. Let us make this more rigorous and prove this result formally. 

We will prove a slight relaxation of the full result; specifically, we will assume that $X$ is a *finite* random variable. However, the result does hold more generally for any random variable. The proof of the more general case involves some fairly heavy machinery and thus, in the interest of clarity and accessibility, we chose to prove this simpler result instead. If you would like to discuss the proof of the more general result, please reach out to us directly via email; we'd be more than happy to talk about it.

*Proof:* Recall that if $f$ is a convex function, then

$$
\begin{equation}
    f(\lambda x + (1-\lambda) y) \leq \lambda f(x) + (1-\lambda) f(y) \tag{1}
\end{equation}
$$

holds for all $x$,$y$ (in the domain of $f$) and $\lambda\in [0,1]$. We first show, by induction, that this result can be generalized to hold for any arbitrary (finite) number of points. Specifically, we will show that if $f$ is a convex function, then

$$
\begin{equation}\label{induc_hyp}
    f\left(\sum_{i=1}^n \lambda_i x_i\right) \leq \sum_{i=1}^n \lambda_i f(x_i) \tag{2}
\end{equation}
$$

holds for all $x_1, \ldots, x_n$ and $\lambda_1, \ldots, \lambda_n \in [0,1]$ with $\sum_{i=1}^n \lambda_i = 1$.

The base case, for $n=2$, holds by the definition of convexity, Eq. (1). Now assume that the result holds for $n=k-1$. We will show that the result also holds for $n=k$. Note that we can write

$$
\begin{align}
    f\left(\sum_{i=1}^k \lambda_i x_i\right) &= f\left(\left(\sum_{i=1}^{k-1} \lambda_i x_i\right) + \lambda_k x_k \right)
\end{align}
$$

Now, we want to split this term to arrive at a term with just $f\left(\sum_{i=1}^{k-1}\lambda_i x_i\right)$, so that we may apply the inductive hypothesis, Eq. (2), which will just about complete the proof. Consider the summation term inside the function call, $\left(\sum_{i=1}^{k-1} \lambda_i x_i\right)$, as a single term. Note that this line looks rather similar to the definition of convexity Eq. (1) in that there are two terms summed together; the only difference is that there does not exist a pair of parameters, which sum to one, accompanying these terms. However, there is an easy fix to this. Let $\lambda_k$ remain as the parameter accompanying $x_k$, and let us pull out a factor of $(1-\lambda_k)$ from the summation term, i.e., we rewrite the line as:

$$
\begin{align*}
    f\left(\sum_{i=1}^k \lambda_i x_i\right) &= f\left(\left(\sum_{i=1}^{k-1} \lambda_i x_i\right) + \lambda_k x_k \right)\\
    &= f\left((1-\lambda_k) \left(\sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k} x_i\right) + \lambda_k x_k \right)
\end{align*}
$$

Now you have, as desired, two terms which are accompanied by a pair of parameters summing to one. We can now apply the definition of convexity to split these two terms and then apply the inductive hypothesis on the summation term to conclude the proof: *(Continued from above)*

$$
\begin{align*}
    f\left(\sum_{i=1}^k \lambda_i x_i\right) &= f\left((1-\lambda_k) \left(\sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k} x_i\right) + \lambda_k x_k \right)\\
    &\stackrel{a.}\leq (1-\lambda_k)f\left(\sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k} x_i\right) + \lambda_k f( x_k)\tag{*}\label{apply_convexity}\\
    &\stackrel{b.}\leq \sum_{i=1}^{k-1} \lambda_i f( x_i) +\lambda_k f(x_k)\\
    &= \sum_{i=1}^{k} \lambda_i f(x_i)
\end{align*}
$$

*a. By applying the definition of convexity, Eq. (1).<br>
b. By applying the inductive hypothesis, Eq. (2).*

Therefore, we have, as desired, that

$$
\begin{equation}
    f\left(\sum_{i=1}^n \lambda_i x_i\right) \leq \sum_{i=1}^n f(\lambda_i x_i).
\end{equation}
$$

Now, the remainder of the proof follows straightforwardly by applying this result to $\mathbb{E}[f(X)]$:

$$
\begin{align*}
    \mathbb{E}[f(X)] = \sum_x P(x) f(x) \stackrel{a.}\leq f\left(\sum_x P(x) f(x)\right) = f(\mathbb{E}(X))
\end{align*}
$$

*a. By assumption, there are only a finite number of $x$'s for which $P(x) \neq 0$ (since $X$ is a finite random variable). Therefore, we can think of each of these $P(x)$ terms as being equivalent to the $\lambda_i$ terms from earlier since $\forall x$, $P(x) \in [0,1]$ and $\sum_x P(x) = 1$. Therefore, we can apply the result from Eq. (2).* $\square$

### When is Jensen's Inequality an Equality?

Let us once again consider the variance of a random variable: (Once, again let $f(x) = x^2$)

$$
\begin{align}
Var(X) = \mathbb{E}X^2 - (\mathbb{E}X)^2 = \mathbb{E}[f(X)] - f(\mathbb{E}X).
\end{align}
$$

We know that the variance of any random vairable is zero if an only if the random variable is constant. In other words, $\mathbb{E}[f(X)] = f(\mathbb{E}X)$ if and only $X$ is constant. One might ask if, as was the case for Jensen's inequality, this result holds for the class of all convex functions.

Unfortunately, this is not the case, but it *almost* is:

***Theorem:** Let $X$ be a finite random variable and $f$ be a strictly convex function. Then, $\mathbb{E}[f(X)] = f(\mathbb{E}X)$ if and only if $X$ is constant.*

*Proof:* It is clear that if $X$ is constant, $\mathbb{E}[f(X)] = f(\mathbb{E}X)$ must hold since we must have that $X = \mathbb{E}X$.

Now, we show that if $\mathbb{E}[f(X)] = f(\mathbb{E}X)$, then $X$ must be constant. We proceed by contradiction. Assume that $X$ is not constant. Now, note that if $f$ is strictly convex and $X$ is a (non-constant) random variable, the result from Theorem 1 can be written as $\mathbb{E}[f(X)] > f(\mathbb{E}X)$. This is because, since $f$ is strictly convex, the inequality in Eq. (\ref{apply_convexity}) in the proof would be strict as well; therefore it follows that the inequality in the theorem itself must be strict.

Thus, where $X$ is not constant, we have that $\mathbb{E}[f(X)] < f(\mathbb{E}X)$ and also, by assumption, that $\mathbb{E}[f(X)] = f(\mathbb{E}X)$. This is a contradiction and therefore, $X$ must be constant. $\square$

## Applications of Jensen's Inequality

We introduced Jensen's inequality as being a "widely-used" result, so this post would not be complete without a discussion of the applications of Jensen's inequality. Specifically, we will discuss two other key applications of Jensen's inequality; specifically, we will see how it can be used to prove the AM-GM inequality and the Rao-Blackwell theorem. 

### An Alternate Proof of the the AM-GM inequality

Recall the elementary result of the relationship between the arithmetic mean and the geometric mean. (You probably last saw this somewhere in high school.)

***Theorem:** (AM-GM Inequality): Let $x_1,x_2,...,x_n$ be non-negative real numbers. Then,
$$
\begin{align}
    \frac{x_1 + x_2 + \ldots + x_n}{n} \geq (x_1 x_2 \ldots x_n)^{1/n}
\end{align}
$$*

*Proof:* Notice that in Jensen's Inequality we relate the expected value of the function to the function of the expected value. Consider a random variable X that is uniform on the set $\{x_1,x_2,\ldots,x_n\}$. Then, the arithmetic mean is just $\mathbb{E}X$.

Now, note that $f(x) = \log(x)$ is a concave function, and so we have 

$$
\begin{align*}
    \log\left(\frac{x_1+x_2 + \dots x_n}{n}\right) = \log(\mathbb{E}X) &\stackrel{a.}\geq \mathbb{E}[\log(X)] = \frac{\log(x_1)+\log(x_2)+\ldots+\log(x_n)}{n} = \log(({x_1 x_2 \ldots x_n})^{1/n})\\
    \Rightarrow \frac{x_1+x_2 + \dots x_n}{n} &\geq (x_1 x_2 \ldots x_n)^{1/n}
\end{align*}
$$

*a. By applying Jensen's Inequality.*
$\square$

### The Rao-Blackwell Theorem

***Prerequisites:*** *The following discussion of the Rao-Blackwell Theorem assumes basic knowledge of statistics, especially with regard to estimators and sufficient statistics.*

The Rao-Blackwell Theorem provides a principled way to improve the performance of an estimator by using a sufficient statistic, with performance based on the mean squared error criterion or some other risk function. 

We now quickly review the definitions of an estimator and a sufficient statistic.

An **estimator** is a statistic that estimates some population parameter using sample data. For example, the sample mean is an estimator for the population mean. 

Let $X$ denote some sample $\{x_1, x_2,\ldots, x_n\}$. A statistic T is said to be **sufficient** for a parameter $\theta$ if $X\mid T=t$ does not depend on $\theta$ for any $t$. Intuitively, a sufficient statistic is one that retains all the information about $\theta$ that is contained in our sample $X$.

For example, the sample mean is a sufficient statistic for the population mean of the normal distribution, i.e., given the sample mean, we cannot extract any more information from the sample about the population mean. However, the sample median is *not* sufficient for the population mean. Even after knowing the sample median, we can still gather more information from our sample about the population mean, e.g. if there were many large points beyond the median and many small points before, we could infer the population mean is likely higher than the sample median.

Now, onto the theorem itself.

***Theorem:** (Rao-Blackwell Theorem): Let $\widehat{\theta}$ be an estimator of $\theta$ and $\mathbb{E}(\widehat{\theta})^{2} < \infty$. Suppose T is sufficient for $\theta$. Let $\widetilde{\theta} = \mathbb{E}(\widehat{\theta}|T)$.
Then, $MSE(\widetilde{\theta}) \leq MSE(\widehat{\theta})$*

The Rao-Blackwell theorem tells us that for any arbitrary estimator, so long as we can find a sufficient statistic T, we can improve this estimator by calculating the conditional expectation of the estimator on T. This process of improving estimators is known as **Rao-Blackwellization**.

Additionally, note that the improved estimator is as a function of T; what this implies is that if we have an estimator that is *not* a function of a sufficient statistic, then that estimator can be improved upon.

*Proof*: We will prove that the result holds for any risk function with an associated loss function that is convex (the risk function is the expected value of the loss function). Then, since the MSE is the expected squared loss, and is therefore convex, our desired result will follow immediately.

Consider the loss function $L(\theta,u)$ convex in u and assume there exists an estimator $\widehat{\theta}$ of $\theta$ such that the risk function $R(\theta,\widehat{\theta}) < \infty$. Let:

$$
\begin{align}
    \widetilde{\theta} = \mathbb{E}(\widehat{\theta}|T)
\end{align}
$$

We then want to show:

$$
\begin{align}
    R(\theta,\widetilde{\theta}) \leq R(\theta, \widehat{\theta})
\end{align}
$$

We have that $L(\theta,u)$ is convex in u, so:

$$
\begin{align*}
    L(\theta,\widetilde{\theta}) &= L(\theta,\mathbb{E}(\widehat{\theta}|T))\\
    &\stackrel{a.}\leq \mathbb{E}(L(\theta,\widehat{\theta}|T))
\end{align*}
$$

*a. By applying Jensen's Inequality with the convex loss function.*

Now, taking expectations of both sides, we have:

$$
\begin{align*}
    \mathbb{E}(L(\theta,\mathbb{E}(\widehat{\theta}|T))) &\leq \mathbb{E}(\mathbb{E}(L(\theta,\widehat{\theta})|T))\\
    &\stackrel{b.}= \mathbb{E}(L(\theta,\widehat{\theta}))
\end{align*}
$$

*b. Since T is a sufficient statistic for $\theta$.*

The risk function is the expected value of the loss function, so we get:

$$
\begin{align*}
    R(\theta,\mathbb{E}(\widehat{\theta}|T)) &\leq \mathbb{E}(L(\theta,\widehat{\theta}))\\
    R(\theta,\widetilde{\theta}) &\leq R(\theta,\widehat{\theta})
\end{align*}
$$

Finally, letting $L(\theta,\widetilde{\theta}) = (\theta - \widetilde{\theta})^2$ and $R(\theta,\widetilde{\theta}) = \mathbb{E}(L(\theta,\widetilde{\theta}))$ we arrive at the desired result.
$\square$

### Wrapping Up

We hope that this exploration of Jensen's inequality has allowed you to gain a deeper understanding into the intricacies and applications of a seemingly intuitive theorem. Of course, Jensen's Inequality has many more applications beyond those two introduced above. One particularly well-known application of Jensen's inequality is, as we briefly discussed in the introduction, in the derivation of the Expectation-Maximization (EM) algorithm in Machine Learning; we are currently planning on writing a full blog post on this algorithm, its derivation, and its applications at some point in the future, so stay tuned for that!

