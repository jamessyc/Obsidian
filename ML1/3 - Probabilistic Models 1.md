## Intro
Can think of machine learning as probabilistic inference
For example, classification as:
$$
p(y_i | x_i, D)
$$
or: What is the probability that example $x_i$ is in class $y_i$ given training data $D$?

Similarly, for clustering:
$$p(x_i|D)$$
What is the probability of example $x_i$ given data set $D$?

The main idea is that lines are fuzzy. Basically, there is no hard 'yes' or 'no' for something.
### Advantages
- We can make probabilistic predictions without having to be absolutely certain
- Each training example has an incremental effect on the estimated probability
	- I.e. each example will increase or decrease a given probability
- Prior knowledge is combined with observed data to determine the final probability
### Disadvantages
- Need initial knowledge of many probabilities (i.e. Bayes theorem)
	- When we don't know these probabilities, we have to make estimates or assumptions
- Computational costs can be high.
## Probability Basics
In general we are interested in some hypothesis $h$. For example, the class that a specific example belongs to.

We want the most probable hypothesis, or the $h$ that maximises $p(h|D)$. 

### Bayes Theorem
$$
p(h|D) = \frac{p(D|h)p(h)}{p(D)}
$$
Where:
- $p(h)$ = the probability of the hypothesis before we have the data. Prior knowledge, estimates, or assumptions.
	- A.k.a. the **prior** probability
	- E.g.: How probable is it that a student in this class gets an A?
- $p(D)$ = the probability that the data will be observed.
	- I.e. how probable we think that data is without any knowledge about which hypothesis holds.
	- E.g.: How probable is this set of coursework marks before we know whether the student will get an A?
- $p(D|h)$ = the probability of observing the data, given that the hypothesis is true. 
	- A.k.a. the **likelihood**
	- E.g.: When I'm told I'm looking at the coursework of a grade A student, how probable are these marks?
- $p(h|D)$ = the probability of observing the hypothesis, given the data
	- A.k.a. the **posterior** probability
	- E.g.: How that I have seen the coursework grades, how probable is it that this student will get an A?
#### Relationship
$p(h|D)$ increases with $p(h)$ and $p(D|h)$.
$p(h|D)$ decreases with $p(D)$.
#### Example: Coursework Grades
The probability that a student will get an A after we have looked at their coursework: $p(h|D)$.
- Increases with the probability that they will get an A: $p(h)$.
- Increases with the probability that this coursework is that of someone that gets an A: $p(D|h)$.
- Decreases with the probability that anyone can produce this coursework: $p(D)$.
## Bayesian Learning
Bayes theorem allows us to calculate the posterior of $h$ given some data $D$.

We can use this for a learning algorithm that calculate the posteriors of different hypotheses, and outputs the one with the highest probability. This is known as the **Maximum a posteriori** (MAP) hypothesis.

$$
\begin{align}
h_{MAP} &= arg \ max_{h \in H} \ p(h|D)\\
&= arg \ max_{h \in H} \ \frac{p(D|h)p(h)}{p(D)}\\
&= arg \ max_{h \in H} \ p(D|h)p(h)
\end{align}
$$
*Note: We remove $p(D)$ from the equation because it doesn't change the outcome. It is constant for all hypothesis $h \in H$, and removing it will also reduce the number of necessary computations.*

We may also assume that every hypothesis $h \in H$ is equally likely a priori. If that is the case, we can also remove the term $p(h)$, and only consider the **likelihood** $p(D|h)$. 

In this case, we want the **maximum likelihood** hypothesis $h_{ML}$:
$$
h_{ML} = arg \ max_{h\in H} \ p(D|h)
$$
## Naive Bayes Classification
Find a class $v \in V$ that maximises the Bayesian probability. In mathsy terms:
$$
v_{MAP} = arg \ max_{v_i \in V} \ p(v_i | a_1, a_2, ... , a_n)
$$
*Note: When seeing $MAP$, just think maximising posterior.*

Using the previous rearrangement, we can rewrite this as:
$$
v_{MAP} = arg \ max_{v_i \in V} \ p(a_1, a_2, ... , a_n | v_i)p(v_i)
$$
How do we get these values?
We estimate based on training data.
- $p(v_i)$ is the number of times $v_i$ appears in the training data.
	- Basically, how common a class is. Number of times the class appears / total training data.
- $p(a_1, a_2, ... , a_n | v_i)$ is the number of cases that have the class $v_i$ which also have the features $a_1, a_2, ... , a_n$.
	- The number of possible combinations is very large, so *a lot* of data is required to get good estimates.
### Simplification
To get around this issue, Naive Bayes makes some assumptions:
1. Assume position does not matter (if applicable)
	- Basically, it doesn't matter which feature a value appears in. A value that appears in feature 30 is treated the same as if it appears in feature 1.
2. Assume that features are conditionally independent given the target $v$.
	- Meaning that each feature is solely dependent on the class $v$, and not other features.
	- This means that we can calculate the likelihood $p(a_1, a_2, ... , a_n | v_i)$ as the product of all the features and the classes. Or, in mathsy terms: $\prod_j p(a_j|v_i)$.
		- $p(a_j|v_i)$ = Look for all the cases in which $v_i$ occurs, and then compute the proportion which have $a_j$.
	- This requires much less data to be accurate.

So, after the assumptions, we then have:
$$
v_{NB} = arg \ max_{v_i \in V} \ \prod_j p(a_j|v_i) \ p(v_i)
$$
*Note: In cases where the **conditional independence** assumption is satisfied, then we can say that $v_{NB} = v_{MAP}$. However, when it is not, then $v_{NB}$ is often still a good solution (verified experimentally).*

### Notes
- For the above method, you don't get the probability as the final result, as some factors have been omitted. To get the real probabilities back (e.g. for sampling or other purposes), **normalization** is required.
- Because taking the product of several probabilities leaves you with quite a small number, it can lead to integer underflow errors. This can be circumvented by taking the logs instead.
- So far we have computed probabilities by counting (e.g. the number of things that belong to a specific class in a given dataset). However, this becomes less effective when the number is very small, and can lead to overfitting.
	- To circumvent, use a better estimator, the **m-estimate**:
		- $\frac{n_i + m.p}{n + m}$ where $p$ is a prior estimate (typically $\frac{1}{k}$ where $k$ is the number of values a feature can take) and $m$ is a constant used to weigh the prior.
- Another note on counting features: If feature values are not discrete, we can model values as a Gaussian distribution instead.
## Gausssian Mixture Models
A combination of probability models:
![[Pasted image 20250213171334.png]]

In general, we mix $K$ base distributions, such that:
$$
p(x_i) = \sum\limits_{k=1}^K \pi_k p_k(x_i)
$$
where the $\pi_k$ are the mixing weights.

In other words, the probability of an element $x_i$ is the sum of the probabilities assigned to $x_i$ by each of the probability models in the mixture. 

To create a Gaussian mixture, we just substitute $p_k(x_i)$ with the Gaussian formula:
$$
p(x_i)= \sum\limits_{k=1}^K \pi_k N(x_i|\mu_k, \Sigma_k)
$$
Where $\mu_k$ is the mean, and $\Sigma_k$ is the $n \times n$ covariance matrix (whatever that means).  

When all Gaussians are univariate, we have:
$$
p(x_i)= \sum\limits_{k=1}^K \pi_k N(x_i|\mu_k, \sigma_k^2)
$$
where $\sigma_k^2$ is the variance.

Gaussian mixtures are good models for representing a number of normally distributed sub-populations within an overall population. For example, human heights:

![[Pasted image 20250213172221.png]]
## Expectation Maximisation
Given a dataset and $K$ Gaussians, we want to find - or estimate - the means.

To figure out the mean of one of our Gaussians from a datapoint, we first need to know which of the Gaussians our datapoint relates to. This is because if we try and figure out the mean of a Gaussian 'B' with a datapoint from Gaussian 'A', we'll get an incorrect value.

Thus for a datapoint $x_i$ and $K$ Gaussians, we need some way of representing which Gaussian $x_i$ came from. So for each datapoint we can have variables $z_{i0}, z_{i1}, ..., z_{iK}$ where each one is either one or zero, one if $x_i$ came from the corresponding Gaussian, and zero if not. Each of these variables represents one of the $K$ Gaussians (hence why we have $K$ variables), and each datapoint will have its own set of variables (hence the $_i$).

We now need some way to determine which Gaussian a datapoint comes from (i.e. a way to update the values of these variables). This is where Expectation Maximisation comes in.
### Method *(For two Gaussians)*
*Note: For simplicity, we will also assume the Gaussians have the same variance.*
We start with an arbitrary value for the means of the Gaussians, which is our hypothesis:
$$
h = \langle\mu_1, \mu_2\rangle
$$
Assuming the hypothesis is true (which it probably isn't, as they are just arbitrary starting values), we use it to estimate the expected value of each hidden variable, represented mathematically as $E[z_{ij}]$. *Note that in this case, $j = k = 2$ as we only have 2 Gaussians.*

Here is some complicated math that I'm not sure we need to understand:
$$
\begin{align}
E[z_{ij}] &= \frac{p(x=x_i|\mu=\mu_j)}{\sum^2_{n=1}p(x=x_i|\mu=\,u_n)}\\
&= \frac{e^{-\frac{1}{2\sigma^2}(x_i-\mu_j)^2}}{\sum^2_{n=1}e^{-\frac{1}{2\sigma^2}(x_i-\mu_n)^2}}
\end{align}
$$
This is just how we get our expected values for our hidden variables.

We then have to calculate a new hypothesis: $$h'=\langle\mu'_1,\mu'_2\rangle$$
Which is done by calculating the weighted average of all the datapoints. Note that they are weighted by $E[z_{ij}]$, meaning if the expectation / probability that a datapoint belongs to a Gaussian is low, it will contribute less to that Gaussian's mean in our hypothesis.
$$
\mu_j \leftarrow \frac{\sum^m_{i=1}E[z_{ij}]x_i}{\sum^m_{i=1}E[z_{ij}]}
$$
We repeat this until convergence occurs at a local maximum. 

### General Description
Above, we applied the EM algorithm to Gaussian Mixtures, but it can be applied in any situation where we want to estimate parameters $\theta$ that describe a distribution, given only a portion of the data generated by said distribution.

For a set of $K$ Gaussians with the same variance:
$$\theta = \langle\mu_1, \mu_2, ..., \mu_K\rangle
$$and with different variance:
$$\theta = \langle\mu_1, \sigma_1^2, \mu_2, \sigma_2^2, ..., \mu_K, \sigma_K^2\rangle$$
#### Some more bullshit
![[Pasted image 20250214141800.png]]

## K-Means
[[4 - Probabilistic Models 2]]