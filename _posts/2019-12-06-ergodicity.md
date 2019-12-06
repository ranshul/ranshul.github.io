---
title:  "[WIP] Ergodicity"
date:   2019-12-06 00:19:00
categories: ['foundations', 'draft']
primary: Foundations
---
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>

Probability is tricky. The ideas built on top of it serve as good mental fodder. However, with "intuition" and working backwards from observations, we might not be able to completely explain why (or whether) our probabilistic models work. The historic evolution of jargon doesn't help a bit either. 

One of the remarkable successes of probabilistic modelling is in the field of statistical mechanics. In typical experiments, a thermodynamic observable is measured; the computed ensemble average is then compared against it. This methodology has worked so far. 

_So, why does it work?_ Well, an accurate justification of this interpretation of thermodynamics eludes us to this date. A point of contention here is the idea of comparing the statistical ensemble average with the experimental measurement (often considered to be the time average). The dichotomy here is ergodicity. 

In Birkhoff's distilled form, $$ f $$ is ergodic if 
<center>$$ \lim_{T\to\infty}\dfrac{1}{T}\int_{0}^{T} f(x(t))\mathrm{d}t = \int \rho (x)f(x)\mathrm{d}x $$</center>
That is, time and space averages must be the same almost everywhere. The measure theoretic technicalities, along with others that plague Ergodic theory, will not be discussed here. The time average here is as $$ t \rightarrow \infty $$. 

It's not obvious that experimentally measured observable is this same time average. In fact, it simply isn't true always. A simple spin system of 1000 particles can force even the most efficient physical process to traverse all states within the universe's predicted lifetime. This gets us to Landau/Lifshitz's statistical independence argument (with the sum functions being approximately constant over the energy hypersurface). However, I digress.

In economics, some of these ideas have been borrowed. The classic wealth from gambling example demonstrates this difference. It goes as follows: The money you have is $$ m(t) $$ for time $$ t $$. For each timestep $$ \delta t $$, your money either doubles or halves with equal probability. Call this multiplicative factor 
<center>$$ r(t) = \begin{cases} 2, \text{with probability } \frac{1}{2} \\ \frac{1}{2}, \text{with probability } \frac{1}{2} \end{cases}$$ </center>
This scenario can be modelled as $$ m(t) = r(t)m(t-\delta t) $$. This can be deparameterized on time by taking the ensemble average: $$ \left\langle x(t + T\delta t)\right\rangle = x(t) \left\langle r \right\rangle^{T} $$. Here, $$ \left\langle r \right\rangle = 2 \times 0.5 + 0.5 \times 0.5 = 1.25 $$ The expectation value (or ensemble average) says that, on the long run, the money in hand increases. Here's the caveat.

> A random variable $$ X $$ **need not** take the value _expectation value_ $$ E[X] $$ or $$ \left\langle X \right\rangle $$ often[^1].

The term expectation value, or mathematical expectation, is an unfortunate misnomer. Its [history](https://en.wikipedia.org/wiki/Expected_value#Etymology) explains the meaning succinctly. 
> If you expect $$ a $$ or $$ b $$, then your expectation value is $$ \dfrac{a+b}{2} $$. However, you would still get either $$ a $$ or $$ b $$.

The other deparameterization is by taking the time average[^2]. Generalizing the multiplicative factor, or the reward, we can have 
<center>$$ r(t) = \begin{cases} r_1, \text{with probability } p \\ r_2, \text{with probability } 1-p \end{cases}$$ </center>
If $$ r_1 $$ occurs $$ n_1 $$ times and $$ r_2 $$ occurs $$ n_2 $$ times[^3], for $$ T $$ timesteps,
<center>$$ \left\langle r \right\rangle_\tau = \lim_{T\to\infty}\left(\dfrac{x(t+T\delta t)}{x(t)}\right)^{\frac{1}{T}} = r_{1}^{n_1}r_{2}^{n_2}$$ </center>
In our particular example, the time average is $$ 1 $$ which indicates that over time, the process does not add any money.

While my notation is probably off, the first "expectation value" gets the name large-ensemble limit in economics. The second one is the long-time limit. Both these averages are two different pieces of information. It is upto the modeller to choose what's required where.

[^1]: This implicitly introduces the frequency of occurence or a temporal aspect of X taking a value.
[^2]: "Zooming out" of the curve enough for one process to filter out the noise and get the trend.
[^3]: Here, $$ n_1 + n_2 = T $$
