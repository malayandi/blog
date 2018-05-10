---
layout: post
title:  "Inverting the Poisson Process"
excerpt: "If A implies B, does B imply A? In the case of the Poisson process, yes."
date:   2018-05-10 00:00:00 +0000
categories: "probability"
---

_**Prerequisites**: This post assumes knowledge of the basics of probability theory (e.g. independence, densities, law of total probability), and of the [Poisson](https://en.wikipedia.org/wiki/Poisson_distribution) and [Exponential](https://en.wikipedia.org/wiki/Exponential_distribution) distributions. Familiarity with multivariable calculus and the Poisson Process will be helpful._

I've taught Berkeley's standard probability course, [Stat 134](http://www.stat134.org), a couple of times and one of my favorite topics in this course is the Poisson process. The Poisson process is an example of a stochastic process, i.e., it models the behavior of a sequence of random events over time. It is one of the simplest examples of a stochastic process. Each event occurs completely at random and is identical to, but does not affect the behavior of, any other event; all we know is the average rate at which these events occur.

The Poisson process can be used, for example, to model the lifespan of light bulbs in your home. (In this case, an event would be a light bulb burning out.) Everytime a light bulb burns out, you replace it with an identical bulb; you expect the average number of light bulbs you use each year to be roughly close to the long-run average but naturally, you expect some variation due to how your usage of the light bulb; and finally, there is no real reason to think the lifespan of one bulb should affect the lifespan of any other. The Poisson process is frequently applied across a variety of fields and has been used to model, among other things:

* in queuing theory, phone calls arriving at a call center [[1]](https://www.unc.edu/~haipeng/publication/EROM-call-center.pdf);
* in geology/ seismology, earthquakes in a region of moderate seismic activity [[2]](http://basin.earth.ncu.edu.tw/download/courses/seminar_MSc/2009/1105-2_A%20review%20of%20earthquake%20occurrence%20models%20for%20seismic%20harzard%20analysis.pdf);
* in economics, companies entering a competitive market [[3]](http://web.uconn.edu/ciom/Kimenyi/Papers/kimenyi%20ptr%20poisson%20probability%20model%20of%20entry.pdf).

<!--**Students generally struggle with this topic when they first encounter it but, I don't think this is due to the difficulty or complexity of the subject matter. (The Poisson process is introduced to the students almost immediately after they first learn about continuous probability and is the first place that they encounter a system where discrete and continuous probability exist in unison. I found that, most often, it is students' difficulties with continuous probability that trickle down and cause them to struggle with the Poisson process.)
-->
I often describe the Poisson process as being elegant or magical, primarily because of how it so wonderfully balances simplicity and structural wealth. Despite its simple formulation, the Poisson process remains *very* analytically interesting. For example, a large variety of popular distributions (including but not limited to, the Exponential, Gamma, Beta, and of course, Poisson distributions) are intrisically linked to, and can be derived directly from, the Poisson process. 

In this post, we will discuss one property of the Poisson process that I am especially fond of: that the traditional definition of the Poisson process can be inverted. (More on what this specifically means later.)


## The Traditional Definition

![Figure 1]({{site.url}}/assets/images/pp_post/fig1.png)

_**Notation**: In the context of Poisson processes, we refer to an event occuring as an arrival and use:_

* $X_j$ _to denote the_ $j^{th}$ _arrival;_
* $W_j$ _to denote the waiting time between_ $X_{j-1}$ _and_ $X_{j}$_;_
* $T_j$ *to denote the total waiting time until* $X_j$*;*
* *and* $N_{(s,t)}$*,* $s< t$ *to denote the number of arrivals in an interval* $(s,t)$*.*

_See Figure 1 for details._


Traditionally, in introductory/ intermediate probability courses, the _Poisson Process with rate $\lambda$_ is formally defined on the positive real line $(0, \infty)$ as a process where:

* the number of arrivals in any interval of length $t$ has a $Poisson(\lambda t)$ distribution, i.e., $N_{(s,s+t)} \sim  Poisson(\lambda t)$;
* and, the numbers of arrivals in disjoint time intervals are independent, i.e., for $s_1 \leq t_1 \leq s_2 \leq t_2$, the number of arrivals in $(s_1, t_1)$ is independent of the number of arrivals in $(s_2, t_2)$.

Using this definition, we can show that (1) the time between any two consecutive arrivals (the waiting time) has a $Exponential(\lambda)$ distribution and (2) that all the waiting times are independent of each other.

We now show (1). Consider an arbitrary arrival $X_j$ that arrives at some time $s$. We will show that $W_j$ is distributed according to an $Exponential(\lambda)$ distribution by showing that they have the same survival function (i.e., $P(W_j > t)$).

![Figure 2]({{site.url}}/assets/images/pp_post/fig2.png)

As shown in Figure 2, if there are no arrivals in the interval $(s, s+t)$, the waiting time $W_j$ must be greater than $t$. Thus,

$$
\begin{align}
P(W_j > t) &= P(N_{(s,s+t)} = 0)\\
&=e^{-\lambda t}.
\end{align}
$$

We therefore have as desired: $W_j\sim$ $Exponential(\lambda)$.

To build some intuition for why (2) holds, consider Figure 1 again. Look at the total arrival times $T_3$ and $T_4$. $T_3$ being longer necessarily means that $T_4$ will be longer since $T_4$ effectively contains $T_3$. (Note that we can write $T_4 = T_3 + W_4$.) They are clearly not independent since the length of one affects the length of the other. As we can see in the diagram however, there is no obvious relationship between any two waiting times, i.e., there is no clear reason why the length of one waiting time should affect the length of any other. One might therefore intuitively suspect that the waiting times are independent and indeed, they are.

The formal proof of (2) is not terribly difficult, but is rather notationally dense. Thus, in the interest of brevity and clarity, I have elected to not include it here. However, it is a good exercise in proof writing and a good test of your understanding of Poisson processes to attempt the proof yourself. Feel free to post in the comments or reach out to me directly [via email](mailto:malayandi12@gmail.com) if you need any help getting started/ if you get stuck.


## Inverting the Definition

We now know that any system where (1) the number of arrivals in any time interval is Poisson also has the property that (2) the time between arrivals are independent Exponentials; i.e., (1) implies (2). A natural question to ask is, "Is the converse true?", or "Does (2) imply (1) back?". In other words, what happens if we invert the definition of the Poisson process and start with (2) instead of (1): if we start with a system where the time between any two arrivals are independent Exponentials, are the number of arrivals in any time interval Poisson? As suggested by the title of this post, the answer is yes!

We will prove a version of this result that retains the essence of the problem while being simpler and more intuitive. Specifically, we will show that the number of arrivals in any time interval beginning at 0 (as opposed to any time interval) is distributed according to a $Poisson(\lambda t)$ distribution. I do this, once again, in the interest of brevity and clarity but will outline at the end of this post, how to prove the full result.

Formally, we define the system as follows: given a sequence of independent and identically distributed $Exponential(\lambda)$ random variables $W_1, W_2, W_3, \ldots$, let $T_j = W_1 + W_2 + \ldots + W_j$ denote the time of the $j^{th}$ arrival. Let $N_t = N_{(0,t)} = \max_{T_j < t} j$ be the number of arrivals in $(0,t)$. We will show that $N_t \sim Poisson(\lambda t)$.

<!-- First, we need to identify the possible values of $N_t$. We note that if $W_1> t$, then $N_t = 0$ and if each of the $W_1 = W_2 = \ldots= 0$, we would have $N_t = \infty$. Thus, it is not too hard to see that $N_t$ can take any non-negative integer value. Now, we need to find $N_t$'s probability mass function.  -->We will proceed slowly to build some intuition. We will first find the probability that there are 0, 1, and 2 arrivals in the interval $(0,t)$, and show that it is equivalent that a $Poisson(\lambda)$ distribution takes values 0, 1, and 2 respectively; finally, leveraging the insight from these cases, we will show the desired, general result.

### Case 0

Consider some arbitrary interval $(0,t)$. First, let's find the probability that there are no arrivals in $(0,t)$, i.e. that $N_t = 0$. Note that the only information we are given, and therefore allowed to use, is the fact that the waiting times are independent and identically distributed $Exponential(\lambda)$ random variables; we must therefore reduce this problem into one dealing with waiting times. We do so as follows: if there are no arrivals in $(0,t)$, then the first arrival _must_ have arrived after $t$ and therefore the first waiting time $W_1$ _must_ be greater than $t$. Thus, $$P(N_t = 0) = P(W_1 > t) = e^{-\lambda t}.$$
This result is what we would expect if $N_t\sim Poisson(\lambda t)$. Great, we're on the right track.

### Case 1

_**Note**_: _Recall that the probability that a continuous random variable $X$ takes any specific value is 0, i.e. $P(X = x) = 0$. (This is (sorta) because there are an infinite number of real numbers in any continuous interval: since the probabilities associated with each of these numbers must sum to one, they must be effectively infiticeminal to begin with.) To overcome this, we will compute $P(X \in (x, x+dx)) = P(X \in \Delta x)$ and take the limit as $dx \rightarrow 0$. (We henceforth use $\Delta x$ refers to the interval $(x, x+dx)$)._

Now, let's find the probability that there is exactly one arrival in $(0,t)$. This means that the time of the first arrival $T_1$ (and therefore the first waiting time $W_1$) must be at some point less than $t$; for now, let's say that the first arrival is near $x$ (i.e. $x\in \Delta x = (x, x+dx)$). The second arrival must however be at some point greater than $t$; as shown in Figure 3, this means that the second waiting time $W_2$ must be greater that $t-x$. (Technically speaking, we require that $W_2 > t-x-dx$ but we can ignore the $dx$ term since it will vanish when we take the limit $dx\rightarrow 0$.)

![Figure 3]({{site.url}}/assets/images/pp_post/fig3.png)

Now, what is this mysterious $x$ value that $W_1$ takes? The answer is that $x$ could be anything between $0$ and $t$; as long as $x$ falls within this interval and the $W_2$ is greater than $t-x$, we will have exactly one arrival in this interval. Thus, by the Law of Total Probability, we have to compute the probability that $W_1$ is near $x$ and $W_2$ is greater than $t-x$ for _every possible value of $x$_ in the interval $(0,t)$, and then sum all these probabilities up to arrive at the desired answer: the total probability that we have exactly one arrival in this interval. Since we are dealing with continuous probabilities, we replace the sum with an integral and proceed as follows:

$$
\begin{align}
	P(N_t = 1) &= \int P(T_1\in \Delta x, T_2 > t)\\
	&= \int_0^t P(W_1\in \Delta x)\cdot P(W_2>t-x)\\
	&= \int_0^t f_{W_1}(x)\cdot P(W_2>t-x)\ dx\\
	&= \int_0^t \lambda e^{-\lambda x} \cdot e^{-\lambda (t-x)}\ dx\\
	&= e^{-\lambda t} \lambda\int_0^t\ dx\\
	&= e^{-\lambda t} \lambda t = e^{-\lambda t} \frac{\lambda t}{1!}.
\end{align}
$$

As suggested by my final line of work, this is again what we would expect if $N_t\sim Poisson(\lambda t)$. 

### Case 2

Let's finally look at one slightly harder iteration of this problem before doing the general case; now, we will find the probability that there are exactly two arrivals in $(0,t)$. As with before, we require that that the time of the first arrival, $T_1$ (and therefore also $W_1$), is near some $t_1$, where $t_1 < t$ (we change the name here for notational convenience). We similarly require that $T_2$ is near some $t_2$, where $0 < t_1 < t_2 < t$; this means that we require that $W_2$ is near $(t_2 - t_1)$. We finally require that $T_3$ be greater than $t$; therefore, we require that $W_3$ be greater than $(t-t_2)$. (See Figure 4 for a visual description of this.) We will once again integrate over all the possible values of $t_1$ and $t_2$ to compute the total probability. (So yes, we do have a double integral this time around!)

![Figure 4]({{site.url}}/assets/images/pp_post/fig4.png)

The bounds of this integral are a little tricky. We will first integrate over $t_1$, considering $t_2$ to be some fixed value. (This is important; the problem becomes annoyingly difficult if you choose to integrate in some other order.) $t_1$ can take any value between $0$ and $t_2$, so these are the bounds we will integrate $t_1$ over. Now, onto $t_2$. Recall that $t_2$ is bounded as follows: $0< t_1 < t_2 < t$, i.e., $t_2$ is bounded between $t_1$ and $t$. However, since we have marginalized $t_1$ already, we need no longer consider it when we integrate over $t_2$; it has, for all intents and purposes, been dealt with. Thus, we take the next tightest bound for $t_2$ and find that $t_2$ can take on values from $0$ to $t$. We thus proceed as follows:

$$
\begin{align}
	P(N_t = 2) &= \int P(T_1\in dt_1, T_2 \in dt_2, T_3 > t)\\
	&= \int_0^t \int_0^{t_2} f_{W_1}(t_1)\cdot f_{W_2}(t_2-t_1)\cdot P(W_3>t-t_2)\ dt_1\ dt_2\\
	&= \int_0^t \int_0^{t_2} \lambda e^{-\lambda t_1} \cdot \lambda e^{-\lambda (t_2-t_2)} \cdot e^{-\lambda (t-t_2)} \ dt_1\ dt_2\\
	&= e^{-\lambda t} \lambda^2 \int_0^{t_2}\int_0^t \ dt_1\ dt_2\\
	&= e^{-\lambda t} \lambda \left(\frac{t^2}{2}\right) = e^{-\lambda t} \frac{(\lambda t)^2}{2!}.
\end{align}
$$

Voila! We get exactly what we expect again. Using what we learnt from these examples, we can go ahead and find $P(N_t = n)$ for some arbitrary $n$ to show that $N_t\sim Poisson(\lambda t)$.

### The General Case

If there are exactly $n$ arrivals in $(0,t)$, we must have that the time of the first $n$ arrivals, $T_1, \ldots, T_n$ is near $t_1, \ldots, t_n$ respectively, and that the $(n+1)^{th}$ arrival is after $t$, i.e. that $T_{n+1} > t$. Translating this into the language of waiting times and integrating over bounds derived similarly to above, we find:

$$
\begin{align}
	P(N_t = n) &= \int P(T_1\in dt_1, \ldots, T_{n-1}\in dt_{n-1}, T_n \in dt_n, T_{n+1} > t)\\
	&= \int_0^t \int_0^{t_n} \ldots \int_0^{t_2} f_{W_1}(t_1)\cdot \ldots \cdot f_{W_n}(t_n-t_{n-1})\cdot P(W_{n+1}>t-t_n)\ dt_1 \ldots dt_{n-1}\ dt_n\\
	&= \int_0^t \int_0^{t_n} \ldots \int_0^{t_2} \lambda e^{-\lambda t_1}\cdot \ldots\cdot \lambda e^{-\lambda (t_n-t_{n-1})} \cdot e^{-\lambda (t-t_n)} \ dt_1 \ldots dt_{n-1}\ dt_n\\
	&= e^{-\lambda t} \lambda^n \int_0^t \int_0^{t_n} \ldots \int_0^{t_2} \ dt_1 \ldots dt_{n-1}\ dt_n\\
	&= e^{-\lambda t} \lambda^n \left(\frac{t^n}{n!}\right) = e^{-\lambda t} \frac{(\lambda t)^n}{n!}.
\end{align}
$$

Thus, we have as desired, that $N_t$ has a $Poisson(\lambda t)$ distribution.

There you go! We have shown that the definition of the Poisson process can be inverted. Well, almost. To complete the proof, we need to show that (1) the above result holds for any arbitrary interval $(s, s+t)$ instead of only intervals of the form $(0,t)$ and (2) the number of arrivals in disjoint intervals are independent. Both of these results can be shown true by exploiting the memoryless property of the Exponential distribution and the result above. (Once again, I think this is worth doing if you have not done this before.)

## Wrapping Up

This result shows that there are actually multiple, _equivalent_ ways of defining the Poisson process. You can define the Poisson process as a process where either (1) the number of arrivals in any time interval are Poisson or (2) the time between intervals is Exponential. (There is in fact a third equivalent definition. See the Appendix for details.) However, it does not matter which definition you choose: the characteristics of the resulting process are the same. i.e., whichever definition you start with, you get the properties specified by the other definition(s) for free!

<!-- I found this result so remarkable, and the proof of it, so beautiful that in Fall 2017, I wrote a question for the Stat 134 Final Exam, in which the student had to derive this result with the help of nothing but a few hints. In hindsight, this was probably too difficult of a question, even by Stat 134's standards - if I recall correctly, the average score on this problem was 15$\%$ and even that was after I graded _very_ leniently.
 -->
As I've said before, the Poisson process is a wonderfully elegant stochastic model and there are plenty more of these beautiful results hidden within it. I highly recommend exploring the Poisson process on your own if you haven't already but just in case you want somewhere concrete to get started, here's another fun one: conditional on the fact there are $n$ arrivals in the interval (0, 1), what is the distribution of the time of the $k^{th}$ arrival, for $1\leq k \leq n$? What if we condition on there being $n$ arrivals in some arbitrary interval $(s, s+t)$ instead?

Please do leave your thoughts and questions in the comments section below, or send them to me directly [via email](mailto:malayandi12@gmail.com). I will try to get back to you as soon as possible. Your feedback is very much appreciated. Until next time :)

## Appendix

**_Bonus:_** There is in fact a third equivalent definition of the Poisson process: as a special case of a [pure birth process](https://en.wikipedia.org/wiki/Birth–death_process).

Consider a process where (1) in any small interval $dt$, there may be at most one arrival; (2) this arrival occurs with some constant probability $\lambda\ dt$; (3) and this arrival is independent of all other arrivals outside this small interval. Formally, let $N_t$ denote the number of arrivals in $(0,t)$. $N_t$ is such that:

* $P(N_{t+dt} = n+m\mid N_T = n) = \begin{cases} 
      \lambda\ dt + o(h) & m = 1 \\
      o(h) & m>1\\
      1 - \lambda\ dt +o(h) & m=0 
   \end{cases}$
* for $s< t$, the increment $N_t - N_s$ is independent of all arivals prior to $s$.

(This definition is similar to that presented in [Probability and Random Process](https://www.amazon.com/Probability-Random-Processes-Geoffrey-Grimmett/dp/0198572220/ref=sr_1_1?ie=UTF8&qid=1525124232&sr=8-1&keywords=grimmett+stirzaker) by Grimmett and Stirzaker.)

This process is also (surprise, surprise) a Poisson process! We can show that this definition is in fact equivalent to both the other definitions presented in this blog post; this is left as an exercise to the reader :)



<!-- ### Step 2
We first show that the above result holds if, instead of the specific interval $[0,t]$, we consider any interval $[s, s+t]$. Note that the only thing that differs between these two cases is the first arrival in the interval. In the former case, we knew that the time of the first arrival time was the length of the first waiting time, i.e. $X_1 = W_1$, since the system started at time $0$; if $X_1$ arrives one second into the interval, $W_1 = 1$. However, in the latter case, the time of the first arrival in the interval does not necessarily equal the length of the corresponding waiting time; if the first arrival in the interval, let's call it $X_{j+1}$ is at $s+1$, the corresponding waiting time $W_{j+1}$ could still be greater than $1$ since $X_{j}$ did not necessarily land at $s$.

Once we deal with this issue, the rest of the problem is rather straightforward. The waiting for all arrivals after the first in the interval behave the same in both cases since they are all independent and identically distributed Exponential random variables. All that varies is the name: what we call $W_2$ in the first case, we call $W_{j+2}$ in the second case. Thus, we can deal with all waiting times after the first identically to the way we did above.

Now, let us recall the memoryless property of the Exponential distribution, which states that for an Exponential random variable X,
$$P(x > s+t \mid x > s) = P(x > t).$$
(This can be interpreted as meaning that an Exponential distribution always behaves as it is "new": the probability that it survives some period of time depends only on the length of that period and not on how long it has already been alive.)

Now, let's just say that, as before, we have had $j$ arrivals before time $s$ in this system and that we are waiting for arrival $j+1$. (It does not actually matter what $j$ is since we are only concerned with the number of arrivals in $[s, s+t]$ and not the number of arrivals before $s$ or after $s+t$.) If $X_{j}$ arrived at some time $s - d_{j}$, we _know_ that at time $s$, $W_{j+1}$ is _at least_ as large as $d_{j}$. So, we can find the probability of having $n$ arrivals in $[s,s+t]$ by repeating our math from above, with the only exceptions being that we replace $W_k$ with $W_{j+k}$ (e.g., $W_1$ becomes $W_{j+1}$) and condition on the fact that $W_{j+1} > d_j$. But, by the memoryless property, we can effectively ignore this condition: the distribution of $W_{j+1}$ conditioned on the fact that $W_{j+1} > d_j$ is the same as the distribution of $W_{j+1}$ (and thus also of W_1). 

Thus, all that differs between these two cases is the labels we used in the math. This does not affect the result we obtained and thus we can conclude that $N(s,s+t)\sim Poisson(\lambda t)$ for any $s, t$.

We can show that arrivals in disjoint intervals are independent in much the same way. Consider two intervals, $[s_1, t_1]$ and $[s_2, t_2]$, for $t_1 \leq t_2 \leq t_3 \leq t_4$. One might imagine that it's fairly straightforward to show that the number of arrivals in each interval is independent since the waiting times are themselves independent: the waiting times for arrivals in the first interval don't depend on those for arrivals in the second interval and vice versa. 

The simplest way to try to prove independence here would be compute $P(N(s_1, t_1) = n_1, N(s_2, t_2) = n_2)$ and show that it can be split into $P(N(s_1, t_1) = n_1)\cdot P(N(s_2, t_2) = n_2)$. This should be largely straightforward: the function we need to integrate will just be the product of the function we would have 
had to integrate if we attempted to compute each of the two probabilities separately. We could then simply split the integral into its two components and arrive at the answer we desired. There is one tiny hitch however: If we try to write out the math for $P(N(s_1, t_1) = n_1, N(s_2, t_2) = n_2)$ as we did above, we will require that $X_{n_1+1} > t_1$ and, if we let $X_{j+1}$ denote the first arrival in the second arrival as before, also that $X_{j+1} > s_2$; however, note that it is entirely possible that $j= n_1$ and thus $X_{n_1+1} = X_{j+1}$. There may be some dependence here.

Luckily, the memoryless property comes to our rescue. $X_{n_1+1} > t_1$ implies that $W_{n+1}$ has survived some amount of time $d$. When we deal with $W_{j+1}$, all we have to do is condition $W_{j+1}$ on the fact that $W_{n+1} > d$. If $j!= n_1$, then we can ignore this condition and simply deal with $W_{j+1}$ since $W_{j+1}$ and $W_{n+1}$ are independent by definition. If $j=n_1$, the distribution of $W_{j+1}$ conditioned on the fact that $W_{j+1} > d$ (in this case, $d = s-t_j$) is the same as the distribution of $W_{j+1}$. So, in either case, we end up handling $W_{j+1}$ on its own, independently from $W_{n_1+1}$. We can thus split the integral into two components as desired, concluding the proof.  -->

















