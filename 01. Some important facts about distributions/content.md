# 01. Some important facts about distributions

$public=true$

## Discrete random variables

$acco$
binomial distribution
- counts the number of successes for a variable with two possible outcomes

$gallery$
**probability mass function**

![image](img$xcy3)

- we see the probability mass function for the number of successes **after 10 trials**
- θ determines the probability of success
	- (ideally, we want to *estimate* this θ, when we suspect a binomial distribution is in play)
$/gallery$

$eq$
$$
\operatorname{Binomial}(k|n,\theta) = 
\binom{n}{k} \theta^{k} (1-\theta)^{n-k}
$$

$widec$
𝑛: number of trials  
𝑘: number of successes  
θ: probability of success  
$\binom{n}{k}$: represents the number of ways in which one can choose 𝑘 successes out of 𝑛 trials
$/widec$
$/eq$
$/acco$

$acco$
Bernouilli distribution
- an experiment consisting of only a single trial
- i.e. we can only have a total number of 0 or 1 successes
$/acco$

### The mean and variance of the binomial distribution

mean of 𝑌 / expectation
- $n \cdot \theta$

variance of 𝑌
- $n \cdot \theta \cdot (1 - \theta)$

#### Computing θ and maximum likelihood

estimating θ (= $\hat{\theta}$)
- happens *from the data*
- $\frac{k}{n}$ -> total average successes out of the number of trials
	- = 'observed proportion of succes'
	- = maximum likelihood estimate of the true θ

'maximum likelihood'
- _likelihood_: the value of the binomial distribution function for a particular value of θ

$example$
- 𝑛 = 10 trials, 𝑘 = 7 successes
- What is the probability of observing 7 successes out of 10?

$$
\operatorname{Binomial}(k=7|n=10,\theta) = 
\binom{10}{7} \theta^{7} (1-\theta)^{10-7}
$$

$widec$
$down we depend the PMF on parameter θ
$/widec$

$$
p( y | \theta ) = p( k=7, n=10 | \theta) = \mathcal{L}(\theta)
$$

- $result what is the probability of our data, given the parameter we assume?

$gallery$
![Image](img$avg0)

This likelihood is maximal when θ is set to 0.7.

$info$
What is important about this plot is that it shows that, given the
data, the maximum point is at the point 0.7, which corresponds to
the estimated mean using the formula shown above: $\frac{k}{n}$ = $\frac{7}{10}$.
Thus, the maximum likelihood estimate (MLE) gives us the most
likely value of the parameter 𝜃 given the data. 
$/info$

$warn$
The phrase “most likely” does not mean that the MLE
from a _particular_ sample of data invariably gives us an accurate
estimate of 𝜃. For example, if we run our experiment for 10 trials
and get 1 success out of 10, the MLE is 0.10. We could have
happened to observe only one success out of ten even if the true
𝜃 were 0.5. The MLE would however give an accurate estimate of
the true parameter 𝜃 **as 𝑛 approaches infinity** (= law of large numbers, $see Statistiek voor humane wetenschappen).
$/warn$
$/gallery$
$/example$

### What information does a probability distribution provide?

#### Compute the probability of a particular outcome

$warn$
Discrete case only!
$/warn$

particular outcome
- probability of obtaining a given number of successes

$code$
```r
(prob <- dbinom(5, size = 10, prob = 0.5))
```

$widec$
$down
$/widec$

```
## [1] 0.2461
```
$/code$

#### Compute the probability of 𝑘 or less (more) than 𝑘 successes

cumulative probability
- probability of obtaining a given number of successes, _or less_, _or more_

$code$
**Less than x**

```r
## the cumulative probability of obtaining
## 0, 1, or 2 successes out of 10,
## with theta=0.5:
pbinom(2, size = 10, prob = 0.5, lower.tail = TRUE)
```

- `lower.tail=TRUE` = 2 or _less_

$widec$
$down
$/widec$

```
## [1] 0.05469
```
$/code$

$code$
**More than x**

```r
## the cumulative probability of obtaining
## 2 or more successes out of 10,
## with theta=0.5:
pbinom(2, size = 10, prob = 0.5, lower.tail = FALSE)
```

- `lower.tail=FALSE` = 2 or _more_

$widec$
$down
$/widec$

```
## [1] 0.9453
```
$/code$

$gallery port$
**Cumulative distribution function**

$widec$
![Image](img$fvwz)
$/widec$
$/gallery port$

#### Compute the inverse of the cumulative distribution function (the quantile functoin)

quantile function
- value of variable 𝑘 from the probability value 𝑝

$code$
Value 𝑘 of the outcome is such that the probability of obtaining 𝑘
or less successes is 0.37:

```r
qbinom(0.37, size = 10, prob = 0.5)
```

$widec$
$down
$/widec$

```
## [1] 4
```
$/code$

#### Generating random data from a Binomial(𝑛, 𝜃) distribution

$code$

```r
rbinom(n = 10, size = 10, prob = 0.5)
```

$widec$
$down
$/widec$

```
## [1] 6 5 6 3 1 7 5 6 4 5
```

$info$
You can also generate a Bernouilli distribution if you set `size=1`.
$/info$
$/code$

## Continuous random variables: an example using the normal distribution

$eq$
$$
Y \sim Normal(\mu,\sigma)
$$

$widec$
𝑌 has as PDF the Normal distribution  
𝜎: standard deviation  
𝜇: mean
$/widec$
$/eq$

$warn$
An important assumption we make here is that **each data point
in the vector of data 𝑦 is independent of the others**. Often, we
express this assumption by saying that we have **“independent and
identically distributed data”**.
$/warn$

$eq$
**Probability density function**

$$
Normal(y|\mu,\sigma)=f(y)= \frac{1}{\sqrt{2\pi \sigma^2}} \exp \left(-\frac{(y-\mu)^2}{2\sigma^2} \right)
$$

$gallery$
![Image](img$mj3m)
$/gallery$
$/eq$

*density* function
- not the probability, but rather a non-negative number that is the value of the function 𝑓(𝑦)

|particular outcome|cumulative probability|quantile function|
|---|---|---|
|![Image](img$000h)|![Image](img$usa7)|![Image](img$db2i)|
|`dnorm`|`pnorm`|`qnorm`|

$info$
**Normal distribution information**

95% of
the probability mass is covered by approximately +-1.96
times the standard deviation about the mean. Thus, the range
𝜇 ± 1.96 × 𝜎 will cover approximately 95% of the area under the
curve. 
$/info$

## Other common distributions

### Standard normal

$wide$
$$
\operatorname{Normal}\left( \mu = 0, \sigma=1 \right)
$$
$/wide$

standard normal /  
*normal*(0,1)
- a **special case** of the normal distribution

$info$
One important fact that is relevant for us in this book is that
if 𝑋~1~, … , 𝑋~𝑛~ are independent and identically distributed random
variables from a distribution with mean 𝜇 and variance 𝜎^2^, then, as
𝑛 approaches infinity, the distribution of the transformed random
variable 𝑍 is:

$$
Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}}
$$

$\bar{X}$ is the mean of the random variables $X_1,\dots, X_n$: $\bar{X}=\sum_{i=1}^n \frac{X_i}{n}$.

**Anthe interjection**: if you were to do an enormous number of random experiments trialling the same thing, and you would take the means of all these experiments, the means would be distributed in a normal distribution (given a few conditions). Always.
$/info$

## The uniform distribution

## The Chi-square distribution

$eq$
$$
f(u; \nu)=
\begin{cases}
\frac{1}{2^{\nu/2} \Gamma(\nu/2)}x^{\frac{\nu-2}{2}\exp(-\frac{x}{2})} &  \text{for }x>0 ,\\
0 & \text{otherwise}
\end{cases}
$$

$widec$
𝑣: degrees of freedom  
Γ(): gamma function = Γ(𝑛) = (𝑛 − 1)! ('faculteit')
$/widec$
$/eq$

mean of $\Chi^2_\nu$
- $\nu$

variance of $\Chi^2_\nu$
- $\nu^2$

$gallery port$
$widec$
![Image](img$ufto)
$/widec$
$/gallery port$

### T-distribution

### F-distribution

## Bivariate and multivariate distributions

univariate distributions
- distributions which only hinge on **one** dimension

bi- and multivariate distributions
- distributions which hinge on **two or more** dimensoins

$info$
Understanding bivariate (and, more generally, multivariate) distributions, and knowing how to simulate data from such distributions,
is vital for us because linear mixed models crucially depend on such
distributions.
$/info$

### Example 1: discrete bivariate distributions

$example$
- experiment
- each trial: two data points
	1. Likert acceptability rating
	2. response accuracy to a yes-no question

$gallery port$
$widec$
![Image](img$92u9)
$/widec$
$/gallery port$
$/example$

$widec$
(other information you already know)
$/widec$

### Example 2: continuous bivariate distributions

bivariate distribution
- two random variables 𝑋 and 𝑌
	- each have their own means and standard deviations
	- also: correlation 𝜌 _between_ them

$eq$
**Variance-covariance matrix**

$$
\Sigma
=
\begin{pmatrix}
\sigma _{x}^2  & \rho\sigma _{x}\sigma _{y}\\
\rho\sigma _{x}\sigma _{y}    & \sigma _{y}^2\\
\end{pmatrix}
$$

$widec$
The square ($^2$) on the diagonal is not a mistake.
$/widec$
$/eq$

$result variance-covariance matrix
- a $\\2 x 2$ matrix
- $\sigma_x$, $\sigma_y$ = standard deviations
- $\rho$ = the correlation between the two random variables
- off-diagonals = *covariance* between 𝑋 and 𝑌

$info$
The number of dimensions of the variance-covariance matrix is dependent on the number of variables in the joint distribution. E.g. a distribution with four variables would have a $\\4 x 4$ variance-covariance matrix.
$/info$

$info$
A variance-covariance matrix is (as far as I can tell, up until now) just a tool to display the variance and the correlation in a single matrix. No actual matrix math happens to it.
$/info$

### Generate simulated bivariate (multivariate) data

$example$
**Generating 100 correlated pairs of data**

- correlation $\rho$ = 0.6
- $\mu$ = 0, $\sigma$ = 5, 10

$code$
**Step 1: define a variance-covariance matrix**

```r
library(MASS)
## define a variance-covariance matrix:
Sigma <- matrix(c(5^2, 5 * 10 * .6, 5 * 10 * .6, 10^2),
	byrow = FALSE, ncol = 2
)
```
$/code$

$code$
**Step 2: generate data**

```r
## generate data:
u <- mvrnorm(
	n = 100,
	mu = c(0, 0),
	Sigma = Sigma
)
```
$/code$

$gallery port$
**Correlation**

$widec$
![Image](img$5bth)
$/widec$
$/gallery port$
$/example$

## Likelihood and maximum likelihood estimation

### The importance of the MLE

## Useful R functions relating to univariate distributions

| |Discrete|Continuous|
|---|---|---|
|Example|Binomial(n,𝜃)|Normal(𝜇, 𝜎)|
|Likelihood function|`dbinom`|`dnorm`|
|Probability 𝑃 (𝑌 = 𝑦)|`dbinom`|always 0|
|CDF 𝐹 (𝑦) = 𝑃 (𝑌 ≥ 𝑦) = 𝑝𝑟𝑜𝑏|`pbinom`|`pnorm`|
|Inverse CDF, 𝐹−1(𝑝𝑟𝑜𝑏) = 𝑦|`qbinom`|`qnorm`|
|Generate simulated data|`rbinom`|`rnorm`|

## Exercises

$exercise$
**Exercise 1.1. Practice using the `pnorm` function**

Given a normal distribution with mean 500 and standard deviation 100, use the `pnorm` function to calculate the probability of
obtaining values between 200 and 800 from this distribution.

```r
```
$/exercise$

$exercise$
**Exercise 1.2. Practice using the `pnorm` function**

Calculate the following probabilities. Given a normal distribution
with mean 800 and standard deviation 150, what is the probability
of getting
- a score of 700 or less
- a score of 900 or more
- a score of 800 or more

```r
```
$/exercise$

$exercise$
**Exercise 1.3. Practice using the `pnorm` function**

Given a normal distribution with mean 600 and standard deviation
200, what is the probability of getting

- a score of 550 or less.
- a score between 300 and 800.
- a score of 900 or more.

```r
```
$/exercise$

$exercise$
**Exercise 1.4. Practice using the `qnorm` function**

Consider a normal distribution with mean 1 and standard deviation 1. Compute the lower and upper boundaries such that:

- the area (the probability) to the left of the lower boundary is
0.10.
- the area (the probability) to the left of the upper boundary is
0.90.

```r
```
$/exercise$

$exercise$
**Exercise 1.5. Practice using the `qnorm` function**

Given a normal distribution with mean 650 and standard deviation
125. There exist two quantiles, the lower quantile q1 and the upper
quantile q2, that are equidistant from the mean 650, such that the
area under the curve of the Normal between q1 and q2 is 80%.
Find q1 and q2.

```r
```
$/exercise$


