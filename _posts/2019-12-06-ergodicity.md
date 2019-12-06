---
title:  "[WIP] Ergodicity"
date:   2019-12-06 00:19:00
categories: ['foundations', 'draft']
primary: Foundations
---

Probability is tricky. The ideas built on top of it serve as good mental fodder. However, with "intuition" and working backwards from observations, we might not be able to completely explain why (or whether) our probabilistic models work. The historic evolution of jargon doesn't help a bit either. 

One of the remarkable successes of probabilistic modelling is in the field of statistical mechanics. In typical experiments, a thermodynamic observable is measured; the computed ensemble average is then compared against it. This methodology has worked so far. So, why does it work? Well, an accurate justification of this interpretation of thermodynamics eludes us to this date. A point of contention here is the idea of comparing the statistical ensemble average with the experimental measurement (often considered to be the time average). The dichotomy here is ergodicity. 

In Birkhoff's distilled form, for a transformation to be ergodic, its time and space averages must be the same almost everywhere. The measure theoretic technicalities, along with others that plague Ergodic theory, will not be discussed here. The time average here is as $$ t \rightarrow \infty $$. There can be a lot of difficulty in claiming that the experimentally measured observable is this same time average. A simple spin system can easily have states that even the most efficient physical processes can't traverse within the universe's predicted lifetime. This gets us to Landau/Lifshitz's statistical independence argument (with the sum functions being approximately constant over the energy hypersurface).

A random variable $$ X $$ **need not** take the value _expectation value_ $$ E[X] $$ or $$ \left\langle X \right\rangle $$ often[^2].

