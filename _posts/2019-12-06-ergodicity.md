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

The classic wealth from gambling example demonstrates this difference. It goes as follows: The money you have is $$ m(t) $$ for time $$ t $$. For each timestep $$ \delta t $$, your money either doubles or halves with equal probability. Call this multiplicative factor 
<center>$$ r(t) = \begin{cases} 2, \text{with probability } \frac{1}{2} \\ \frac{1}{2}, \text{with probability } \frac{1}{2} \end{cases}$$ </center>
This scenario can be modelled as $$ m(t) = r(t)m(t-\delta t) $$. To make sense of the model, one approach is to remove the randomness. Typically, this would involve some limiting process that computes an average. The average over configurations (expectation value), also called the ensemble average, is computed as follows: $$ \left\langle x(t + T\delta t)\right\rangle = x(t) \left\langle r \right\rangle^{T} $$. Here, $$ \left\langle r \right\rangle = 2 \times 0.5 + 0.5 \times 0.5 = 1.25 $$ The expectation value seems to indicate that the money in hand increases (exponentially at that). Here's the caveat.

> A random variable $$ X $$ **need not** take the value _expectation value_ $$ E[X] $$ or $$ \left\langle X \right\rangle $$ often[^1].

The term expectation value, or mathematical expectation, is an unfortunate misnomer. Its [history](https://en.wikipedia.org/wiki/Expected_value#Etymology) explains the meaning succinctly. 
> If you expect $$ a $$ or $$ b $$, then your expectation value is $$ \dfrac{a+b}{2} $$. However, you would still get either $$ a $$ or $$ b $$.

Clearly, the actual money after a few timesteps would be in one of the very real configurations, not the "averaged out" scenario  
Another measure is the time average[^2]. Generalizing the multiplicative factor, or the reward, we can have 
<center>$$ r(t) = \begin{cases} r_1, \text{with probability } p \\ r_2, \text{with probability } 1-p \end{cases}$$ </center>
If $$ r_1 $$ occurs $$ n_1 $$ times and $$ r_2 $$ occurs $$ n_2 $$ times[^3], for $$ T $$ timesteps,
<center>$$ \left\langle r \right\rangle_\tau = \lim_{T\to\infty}\left(\dfrac{x(t+T\delta t)}{x(t)}\right)^{\frac{1}{T}} = \lim_{T\to\infty} r_{1}^{n_1/T}r_{2}^{n_2/T} = r_{1}^{p}r_{2}^{1-p}$$ </center>
Here, $$ p = \lim_{T\to\infty} \frac{n_1}{T} $$. In our particular example, the time average is $$ 1 $$ which indicates that over time, the process does not add any money.

Hopefully, this clarifies that the ensemble average and time average are different pieces of information about the same process or system. The difference between the two averages become key in the study of Brownian motion and some interpretations of Itô calculus. To model human decision making, the rate of change of expectation values of money or wealth $$ \frac{\delta m}{\delta t} $$ have been known to be woefully inadequate since early 1700s[^4]. 

The explanation given was that different people treat equal amounts of extra money differently. To fix this, it was proposed that humans tend to look at the rate of change of the expectation value of a "utility" function $$ \frac{\delta u}{\delta t} $$[^5]. The utility function $$ u $$ maps the wealth or money to what, uh, utility the person has for that amount of money. Clearly, this can change from person to person.

I have my qualms with expected utility theory - never got around to understanding it entirely. Utility theory makes the assumption that humans optimize the ensemble average of the utility. In these models, there are two key components
* dynamic - describes wealth growth; could be additive or multiplicative.
* utility - ideally mirrors the importance given to an attribute like wealth in human decision making.

Together, they are expected to model human behavior in a scenario. 

_Narrator_: Here's a function that tells you how you work. 
_Me_: I'm a complex being. No function can capture why I.. uh this seems to be a better way to live life.
_Also me after 5 minutes_: Wait, I don't know myself at all.

Existential nightmares aside, the resolutions proposed to the questions that arise from results of utility theory feel either unnatural or reminiscent of pesky "exceptions" in chemistry. At this point, there are two issues that stick out:
1. Ensemble averages without ergodicity are shady. Even if they're ergodic, observed data and the subsequent analysis on it gives a time average (and not a $$ T\to\infty $$ average). Even in statistical physics, there are cases of symmetry breaking that potentially lead to the breakdown of ergodicity over different time scales.
2. There's a good chance that human behavior results in optimizing something other than utility.

[^1]: This implicitly introduces the frequency of occurence or a temporal aspect of X taking a value.
[^2]: "Zooming out" of the curve enough for one process to filter out the noise and get the trend.
[^3]: Here, $$ n_1 + n_2 = T $$
[^4]: Bernoulli's correspondences, with Montmort and the [St. Petersburg's Paradox](https://en.wikipedia.org/wiki/St._Petersburg_paradox) in this case, are interesting.
[^5]: Expected wealth vs expected utility models.
