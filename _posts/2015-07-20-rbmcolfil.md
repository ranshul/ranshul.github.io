---
title:  "Collaborative Filtering With Restricted Boltzmann Machines"
date:   2015-07-20 22:37:00
categories: ['Recommender Systems']
primary: RecSys
---
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>
<h2> Restricted Boltzmann Machines </h2>
Boltzmann machines are complete graphs with weighted edges between any two nodes in a graph (a type of RNN[^1]). The nodes of the graph encode the features from raw data upon training. They are trained to classify a set of training inputs or generate samples from an input distribution. The visible units encode the input and output. The hidden units capture the relation between the input and output. A probability is assigned to each configuration of hidden and visible units $$h$$ and $$v$$. 

<center>$$ p\left(h,v\right)=\frac{e^{-E(v,h)}}{Z} $$ </center>

Here, $$Z$$ is the normalizing factor [^2] and $$E$$ is the "energy" of the system. It is known that training a complete Boltzmann machine is intractable. Restricted Boltzmann Machines or RBMs have the additional constraint that there can be no intra-layer connection. If there is more than one hidden layer, it becomes a Deep Belief Net or DBN. Taking inspiration from physics, this can be interpreted as an energy minimization problem for an Ising spin system. Accessible details of the math behind this can be found [here](http://deeplearning.net/tutorial/rbm.html) and [here](https://www.cs.toronto.edu/~hinton/absps/guideTR.pdf). A detailed mathematical exposition can be found [here](http://www.iro.umontreal.ca/~lisa/twiki/bin/view.cgi/Public/DBNEquations).

Despite the restrictions imposed in RBMs, calculating the partition function $$Z$$ and training it by gradient descent (backpropagation) is computationally expensive. Contrastive Divergence proposed by [Hinton](http://www.cs.toronto.edu/~fritz/absps/nccd.pdf) is used for for training. 

> Contrastive divergence is a recipe for training undirected graphical models (a class of probabilistic models used in machine learning). It relies on an approximation of the gradient (a good direction of change for the parameters) of the log-likelihood (the basic criterion that most probabilistic learning algorithms try to optimize) based on a short Markov chain (a way to sample from probabilistic models) started at the last example seen.
>
> *Yoshua Bengio* ([Quora](https://www.quora.com/What-is-contrastive-divergence))

To calculate the gradient in the backpropagation equation, the gradient of the log partition function is estimated without actually calculating the log partition function. Running a Markov Chain[^3] with a Gibbs sampler[^4] as the transition operator will take quite a few iterations. It turns out (Hinton's assertion) that with as small as 1 cycle of the [MCMC algorithm](http://www.people.fas.harvard.edu/~plam/teaching/methods/mcmc/mcmc_print.pdf) can be sufficient for the values to converge. 

The Gibbs sampling method is one of the most used Markov Chain Monte Carlo (MCMC) methods. The objective of these algorithms is to sample values given a probability distribution. When there is a known joint distribution $$P(X_1,X_2,\cdots,X_n)$$ where the $$X_i$$ are *independent*, each $$P(X_i)$$ can be independly sampled and a set of values can be obtained for all $$X_i$$. This is valid because the independence of $$X_i$$ for $$i\in\{1,2,3,4,5,6\}$$ lets us factorize the distribution: $$ P(X_1,X_2,\cdots,X_n) = P(X_1)P(X_2)\cdots P(X_n) $$. 

A simpler and more lucid example is as follows: consider the probability distribution of rolling a die. $$P(X=i)=\frac{1}{6}$$ for $$i\in \{1,2,3,4,5,6\}$$. Generate a random number $$r \in [0,1]$$ with uniform distribution. Divide $$[0,1]$$ into 6 equal parts and label them from 1 to 6. Depending on where $$r$$ falls, X gets that label.

In practice, most joint distributions are either unknown or ugly (mathematically). They aren't readily factorized and cause a lot of trouble. Gibbs sampling is an efficient way to approximate the joint distribution provided that $$X_i$$ can be sampled from the conditional distribution $$P(X_i\vert X_1\cdots X_{i-1} X_{i+1}\cdots X_n)$$. The value of $$ X_i $$ in iteration $$k$$ is denoted as $$ X_i^{(k)} $$. It is obtained from $$ P(X_i^{(k)}\vert X_1^{(k)}\cdots X_{i-1}^{(k)} X_{i+1}^{(k-1)}\cdots X_n^{(k-1)}) $$. Incidentally, parallelizing it proves to be [tricky](http://lccc.eecs.berkeley.edu/Slides/Gonzalez10.pdf).

Luckily for us, a Boltzmann machine (hence an RBM) is a form of a [Markov Random Field](http://image.diku.dk/igel/paper/AItRBM-proof.pdf) (MRF). This immediately gives a wealth of theoretical results and algorithms for RBMs. The [Hammersly-Clifford theorem](http://vis.uky.edu/~cheung/courses/ee639/Hammersley-Clifford_Theorem.pdf) reaffirms some key implicit assumptions for mathematicians.

The hidden units and visible units of an RBM are conditionally independent given one-another. As an optimization, block Gibbs sampling is performed. In general, this allows us to to group two or more variables together and sample from their joint distribution. Here, the hidden units are sampled together given the visible units. For an RBM with binary units, the grouped conditional distributions [turn out](http://deeplearning.net/tutorial/rbm.html) to be sigmoids. In that case, after iteration $$n$$, $$h^{(n+1)}$$ is a function of $$v^{(n)}$$ and $$v^{(n+1)}$$ is a function of $$h^{(n+1)}$$. Contrastive Divergence does only $$k$$ steps of Gibbs sampling and in practice, $$k=1$$ tends to work well. Scikit-Learn has RBMs with [binary units](http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.BernoulliRBM.html#sklearn.neural_network.BernoulliRBM).

One drawback of contrastive divergence is that it gives a biased estimate of the gradient. To address this problem, Persisent Contrastive Divergence was introduced. Other algorithms have been proposed for better gradient estimates. But most of them rely on persistent Markov chains and weren't highly relevant to the collaborative filtering section.

The process described in training RBMs naturally makes it a generative model. It "learns" from the pool of training data and attempts to reconstruct data that is similar to the training data. The hidden units can be interpreted as latent factors associated with the input. By now, we know that if we model a problem as an RBM with the conditional probabilities and so on, we can get predictions.

<h2> Collaborative Filtering </h2>
[This](http://www.cs.utoronto.ca/~hinton/absps/netflixICML.pdf) paper applies RBMs to the Netflix Prize. There are $$M$$ movies, $$N$$ users and integer rating values from $$1$$ to $$K$$. If all $$N$$ users rated the same $$M$$ movies, they could be the training data for the RBM. Since that is almost never the case, the design of the RBM must be more general. The paper proposes to use a different RBM for different users. The visible layer will have softmax units for the movies rated by the user. The visible layer will be symmetrically connected to the hidden layer (binary features). The hidden layer will be common to all RBMs and the updates will be reflected across all of them only for the relevant input-hidden layer connections. 

The part about different RBMs for different users continues to pique me. One alternative I can think of is to have all $$M$$ movies in the visible layer. When a user rates $$X\leq M$$ movies, the $$X$$ weights can be chosen while the others are undisturbed. The RBM per user approach mentioned in the paper has the advantage of being parallelizable. In the paper, the full gradients with respect to the shared weight parameters are obtained by averaging over all the users. This can be replicated in the approach with just one RBM - the worker nodes can compute the evenly sliced training batch and the sum of the result emitted can be the total update for that training batch. That said, I don't know if multithreading speeds up the entire process - the threading overhead might make it counterproductive.

The hidden layer captures the latent factors between inputs. The symmetric connections between the hidden and visible layer helps the RBM capture it. For example, one of the RBM's hidden units may denote the comedy genre. If the RBM has been trained with high ratings for comedy movies, that hidden unit has a high probability of getting activated for another comedy movie for a new user who has similar interests. The RBM obviously does not put the label "comedy" on its hidden unit.

The paper goes a step further. It identifies movies viewed by the user but with an unknown rating (given by Netflix as a part of the test set). Normal collaborative filtering models can't harness this information. Neither can the type of RBM we have disccused till now. It turns out that it is possible to use the missing ratings to influence the hidden features of the RBM without reconstructing those ratings. The type of RBM the paper proboses to use is a *conditional* RBM.

In a CRBM, The energy in the conditional probability has an extra energy term that captures the conditional interactions between the input features and the output. In this context, the CRBM takes into account the information by defining a binary vector $$\mathrm{r}\in\{0,1\}^M$$ that shows if a user has rated a particular movie or not. This vector affects the state of the hidden unit. The proposed model is a joint distribution over $$(\mathrm{R},\mathrm{h})$$ conditional on $$\mathrm{r}$$. $$\mathrm{R}$$ denotes the observed ratings.

For further optimization, the paper factorizes the parameter (weight) matrix $$W$$ and the factors are learnt. This is a Conditional Factored RBM. They compare the results with the SVD model and conclude by linearly combining the predictions of different versions of the RBM and SVD methods to achieve an impressive error rate.

Interestingly, the company D-Wave made an adiabatic "quantum" computer which, broadly put, translates the optimal solution to the lowest energy state. Since the Boltzmann Machine problem has been realized to minimizing energies in an Ising spin model, perhaps this is naturally suited. There are a handful of papers on these topics - [Towards the Implementation of a Quantum RBM](http://www.cs.ubc.ca/~nando/papers/quantumrbm.pdf), [Quantum Deep Learning](http://research.microsoft.com/pubs/233053/1412.3489.pdf). 

<h3> Footnotes </h3>

[^1]: Recurrent Neural Network
[^2]: can be interchanged with ***partition*** function. This terminology is borrowed from statistical mechanics.
[^3]: future states are independent of past states given the present state 
[^4]: a nice exposition can be found [here](http://www.umiacs.umd.edu/~resnik/pubs/gibbs.pdf)
