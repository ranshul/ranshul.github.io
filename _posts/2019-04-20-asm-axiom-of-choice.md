---
title:  "Axiom of Choice"
date:   2019-04-20 00:19:00
categories: ['foundations']
primary: Foundations
---

Take all the natural numbers and list them down:
$$ 1, 2, 3, 4, 5, 6, \cdots $$

Now divide them into two lines - odd and even numbers.
$$ \begin{center} 1, 3, 5, 7, 9, \cdots \\ 2, 4, 6, 8, 10, \cdots \end{center}$$

If the first line is $$ \{o_i \in \textrm{Odd}\} $$ and the second line is $$ \{e_i \in \textrm{Even}\} $$, then $$ e_i=o_i + 1 $$ by inspection. Intuitively, it can be concluded that the number of odd and even numbers (though infinity) are the same. To prove this formally, the Schröder–Bernstein theorem[^1] is used.

A quick next step would be to use the bijection $$ f: \textrm{Odd}\to\mathbb{N} = \dfrac{o_i + 1}{2} $$. Now, we have proved that the numer of odd numbers and the number of all natural numbers, that is, $$ |\mathrm{Odd}|=|\mathbb{N}| $$. We also know the following:
1. $$ |\textrm{Odd}|=|\textrm{Even}| $$
2. $$ \textrm{Odd}\cup\textrm{Even}=\mathbb{N} $$
3.  $$ \textrm{Odd}\cap\textrm{Even}=\emptyset $$

Clearly, we're running into arithmetic problems with inifinites. We'd like a way to handle these things. To add "sizes" consistently. 

**Enter measure theory**. Actually, before that, 
sets that have a bijection with the set of natural numbers are said to be *countable* sets[^2]. For the simple reason that if $$ S = \{x, y, z, ...\} $$ you can literally go and count "element $$ x $$ is 1, $$ y $$ is 2" and so on. 

**Measure theory re-enters** Take a countable set $$ S $$. It's fairly straightforward to prove that its measure is 0.

> Let the elements be $$ \{x_i\}^\infty_{i=0} $$. 
> 
> Define a new set $$ A_i =(x_i-2^{-i}\epsilon,x_i+2^{-i}\epsilon) $$ where $$ \epsilon > 0 $$. 
> 
> Now,  $$ S\subseteq\bigcup\limits^\infty_{i=1}A_i $$. The measure of each $$ A_i $$ is 
> 
> $$ m(A_i)=(x_i+2^{-i}\epsilon)-(x_i-2^{-i}\epsilon)=2^{1-i}\epsilon $$. 
> 
> Hence, $$ m(S) \leq \sum\limits_{i=1}^\infty2^{1-i}\epsilon = 2\epsilon $$. 
> 
> Since we can do this for arbitrary $$\epsilon \in \mathbb{R}^+$$ and we can construct $$A^\prime$$ such that $$S\subseteq A^\prime$$ and $$m(A^\prime) \leq \epsilon$$, the measure of $$S$$ is 0.

Now, we can use the measure to "size up" a set. These countable sets can be thought of as "discrete" sets. The set of rational numbers $$\mathbb{Q}$$ can be mapped to $$\mathbb{N}$$ as well. 

The set of real numbers are _uncountable_. Cantor proved it by showing that it's impossible to enumerate all the real numbers in the interval $$[0,1]$$. This makes the cardinality of $$[0,1]$$ an uncountable infinity $$ (2^{|\mathbb{N}|}) $$. Since the interval can be mapped to $$ \mathbb{R} $$, the cardinalities are same. Note that the measures are different. As encountered earlier, for 1D intervals, the measure is the difference in the endpoints.



[^1]: If there is a bijection between sets $$ A $$ and $$ B $$, then the cardinality of the sets are the same. [Wiki](https://en.wikipedia.org/wiki/Schr%C3%B6der%E2%80%93Bernstein_theorem).

[^2]: A set $$ S $$ is countable if and only if there exists an injection $$ f : S \to \mathbb{N} $$. A bijection is a specialized case.