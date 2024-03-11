# 02. Hypothetical repeated sampling and the t-test

$public=true$

## Some terminology surrounding typical experiment designs in linguistics and psychology

----
$widec$
POV: linguistic *experiments* / psychology
$/widec$
----

independent data
- only one data point per participant

dependent data / repeated measures
- multiple measurements from each participant
- _dependency_ between data points, because multiple measurements come from a **common source**

$info$
There's also more information about a Latin Square-design, but that isn't really important at this time.
$/info$

## The central limit theorem using simulation

----
$widec$
Simulation!
$/widec$
----

𝑦
- vector of collected data

𝑁𝑜𝑟𝑚𝑎𝑙(𝜇 = 500, 𝜎 = 100)
- underlying distribution

$result estimation
- we use $\bar{x}$ and $s$ to get an estimation of 𝜇 and 𝜎

$code$
```r
## sample size:
n <- 1000
## independent and identically distributed sample:
y <- rnorm(n,mean=500,sd=100)
## histogram of data:
hist(y,freq=FALSE)
## true value of mean:
abline(v=500,lwd=2)
```

$gallery$
![Image](img$4n1w)
$/gallery$
$/code$

repeated sampling
- a 'thought' experiment -> we cannot do this normally
- we take 100 repeated samples

$code$
```r
mu <- 500
sigma <- 100
## number of experiments:
k <- 2000
## store for data:
y_matrix <- matrix(rep(NA, n * k), ncol = k)
for (i in 1:k) {
	## expt result with sample size n:
	y_matrix[, i] <- rnorm(n, mean = mu, sd = sigma)
}
```
$/code$

$gallery$
![Image](img$qrcr)
$/gallery$

$result *consequence* of repeated sampling
- = Central Limit Theorem
- if we compute the means $\bar{y}_k$ of each of the 𝑘 = 1, …, 100 experiments we just carried out -- and certain conditions are met -- these
means will be **normally distributed**, with mean 𝜇 and standard
deviation $\frac{\sigma}{\sqrt{n}}$

conditions for CLT
- 1. sample size should be "large enough"
	2. 𝜇 and 𝜎 are defined for the probability density
or mass function that generated the data

$result consequences of CLT
- it becomes possible to say something about what the **plausible** and **implausible** values of the sample mean are under repeated sampling
- on the basis of *one* sample we take!

## Three examples of the sampling distribution

## The confidence interval, and what it's good for

standard error
- the (estimated) standard deviation **of the sample means distribution**

$eq$
**Confidence interval**

$$
\text{CI} = \bar{y} \pm 2 SE
$$

$widec$
𝑦: sample data  
$\bar{y}$: sample mean  
𝑆𝐸: $\frac{s}{\sqrt{n}}$
$/widec$

- sampling distribution of means is normally distributed ($see $up)
- 95% of the area under the curve is covered by $\\2 \cdot \text{SE}$
- => the upper and lower bounds of the interval defined by the above formula covers approximately 95% of the area under the curve in the sampling distribution
$/eq$

$info$
The notion of the confidence interval needs to be interpreted as follows: if you take samples repeatedly and compute the CI each time, **95% of those CIs will
contain the true population mean 𝜇**.

```r
mu <- 500
sigma <- 100
n <- 1000
nsim <- 1000
lower <- rep(NA, nsim)
upper <- rep(NA, nsim)
for (i in 1:nsim) {
	y <- rnorm(n, mean = mu, sd = sigma)
	lower[i] <- mean(y) - 2 * sd(y) / sqrt(n)
	upper[i] <- mean(y) + 2 * sd(y) / sqrt(n)
}
## check how many CIs contain mu:
CIs <- ifelse(lower < mu & upper > mu, 1, 0)
## approx. 95% of the CIs contain true mean:
round(mean(CIs), 2)
```

$widec$
$down
$/widec$

```
## [1] 0.95
```

$gallery$
**95% CIs in 100 repeated samples**

![Image](img$3s77)

Illustration of the meaning of a 95 percent confidence interval (CI). The thicker bars represent the CIs which do
not contain the true mean.
$/gallery$
$/info$

### Confidence intervals are often misinterpreted

misinterpretation of the confidence interval
- represents the range of plausible values of the 𝜇 parameter
- /!\ **wrong** -> 𝜇 is a **point value** -> doesn't have its own internal distribution

what good is a CI?
- = a **summary** that tells us the width of the sampling distribution of the mean
- wider = more implied variability under repeated sampling
- => *how uncertain can we be about the estimate of the sample mean under hypothetical repeated sampling?*

## Hypothesis testing: the one sample t-test

t-test
- one of the simplest statistical tests one can do with continuous data

### The one sample t-test

#### Sampling

situation
- random sample 𝑦 of size 𝑛
- data come from a 𝑁(𝜇, 𝜎) distribution
	- but: we don't know 𝜇 and 𝜎

$warn$
For a t test, the data points must be **INDEPENDENT** ($see $up).
$/warn$

$eq$
**Sampling distribution under (hypothetical) repeated sampling**

$$
N(\hat\mu,\frac{\hat \sigma}{\sqrt{n}})
$$

$warn$
The above sampling distribution
is **only as realistic as the estimates of the mean and standard deviation parameters** — if those happen to be inaccurately estimated,
then the sampling distribution is not realistic either.
$/warn$
$/eq$

#### Null hypothesis

$code$
We estimate a random sample of size 1000 from random variable 𝑌 that is normally distributed, with mean 500 and standard deviation 100.

```r
n <- 1000
mu <- 500
sigma <- 100
## generate simulated data:
y <- rnorm(n, mean = 500, sd = 100)
## compute summary statistics:
y_bar <- mean(y)
SE <- sd(y) / sqrt(n)
```
$/code$

null hypothesis testing
- we assume our true sampling distribution of sampling means is centred around a fixed value 𝜇
- e.g. $H_0: \mu = 450$

$gallery$
**The sampling distribution with mu=450**

![Image](img$wpr3)

The sampling distribution of the mean when the
null hypothesis is that the mean is 450. Also shown is the observed
sample mean.
$/gallery$

[ How to do null hypothesis testing? ]
|the sample mean $\bar{y}$ is "near" the hypothesized $\mu$|the sample mean $\bar{y}$ is "far" from the hypothesized $\mu$|
|---|---|
|the data are **possibly** (but by no means necessarily) **consistent** with the null hypothesis distribution|the data are **inconsistent** with the null hypothesis distribution|

$widec$
hypothesised $\mu$ = 450, $see $up
$/widec$

"near" / "far"
- quantified by **how many standard error units** the sample unit is from the hypothesised mean ($see $down)

$eq$
**Distance between sample mean and hypothesised mean**

$$
t \cdot SE = \bar{x} - \mu 
$$

- $result sample mean is 𝑡 standard errors away from the hypothesised mean
$/eq$

$eq$
**Observed t-statistic**

$$
t_{obs}  = \frac{\bar{x} - \mu}{SE}
$$

- if we divide both sides with the standard error, we obtain something that is referred to as the **observed t-statistic**
- = an expression of the distance between the sample mean and the hypothesised mean
	- "how many standard errors is the t-value away from the (presumed) sample mean?"

$warn$
The t-value is a random variable: it is a transformation of $\bar{X}$, the random variable generating the sample means. The
t-value can therefore be seen as an instance of the following transformed random variable 𝑇:

$$
T  = \frac{\bar{X} - \mu}{SE}
$$

This random variable has a pdf associated with it, the t distribution, which is defined in terms of the sample size 𝑛; the pdf
is written 𝑡(𝑛 − 1). Under repeated sampling, the t-distribution is
generated from this random variable 𝑇.
$/warn$
$/eq$

#### Null hypothesis testing 101

1. Define the null hypothesis: in our example, the null  hypothesis was that $\mu = 450$.  This amounts to making a commitment about what fixed value we think the true underlying distribution of sample means is centred at. 

$gallery$
![Image](img$lcc0)
$/gallery$

2. Given data of size $n$, estimate $\bar{y}$, standard deviation $s$, and from that, estimate the standard error $s/\sqrt{n}$.  The standard error will be used  to describe the sampling distribution's standard  deviation.

$gallery$
![Image](img$k58i)
$/gallery$

3. Compute the observed t-value:  
	$t=\frac{\bar{y}-\mu}{s/\sqrt{n}}$
4. Reject null hypothesis if the observed t-value is "large" (to be made more precise next).

$gallery$
![Image](img$y4jn)
$/gallery$

6. Fail to reject the null hypothesis, or (under some conditions, to be made clear later) even go so far as to accept the null hypothesis, if the observed t-value is "small".

"large" or "small"?
- t-value from the sample is large when we end up far in *either* tail of the distribution
	- = the *rejection region*
	- goes off to infinity on the outer sides

$gallery$
![Image](img$j5t3)

- demarcation of the rejection region is as such that the area under the curve (one region) is 0.025
	- this area = probability of observing a value as extreme as the critical t-value (or an even more extreme value)
$/gallery$

#### Identifying the rejection region

rejection region
- 𝑡~critical~ must be defined in such a way that the area from 𝑡 until the tail of the distribution is 0.05 (or 0.025 if symmetrical)
- /!\ dependent on the number of data points! -> t distribution hinges on *degrees of freedom*

$info$
For large
sample sizes, say 𝑛 > 50, the rejection point converges to about 2 (1.96).
$/info$

$code$
**Computing the (absolute) critical t value**

|15 data points|50 data points|
|---|---|
|`abs(qt(0.025, df = 15))`|`abs(qt(0.025, df = 50))`|
|2.131|2.009|
$/code$

#### Easy mode

$code$
**t.test**

The built-in function `t.test` delivers the observed t-value:

```r
## observed t-value from t-test function:
t.test(y, mu = 450)$statistic
```

$widec$
$down
$/widec$

```
## t
## -0.8859
```

$info$
The default value for the null hypothesis mean 𝜇 in this function
is 0; so if one doesn’t define a null hypothesis mean, the statistical
test is done with reference to a null hypothesis that 𝜇 = 0. That
is why this t-value does not match our calculation above:
$/info$
$/code$

#### Null hypothesis mean = 0

zero-centred null hypothesis
- the most common usage of the t-test
- usually, one is comparing a difference in means between two conditions / two sets of conditions

#### Null hypothesis is false?

----
$widec$
What would the t-distribution look like if the null hypothesis were _false_?
$/widec$
----

|H~0~ 𝜇|true 𝜇|
|---|---|
|450|470|

$gallery$
**T-distribution under repeated sampling**


![Image](img$uxf3)

- t-distribution is centred around 2
- if we plug in the hypothesised mean (450) and the true mean (470) and the standard error $\frac{100}{\sqrt{(100)}}=10$ into the equation for computing the t-value, the expected value of the t-distribution (its mean) is 2

$info$
These are the t-values that would appear if we were to repeatedly test a certain hypothesis under the (incorrect) assumption
$/info$
$/gallery$

### Type I, II error and power

#### Errors and worlds

<table>
	<tr>
		<th></th>
		<th>Possible world 1</th>
		<th>Possible world 2</th>
	</tr>
	<tr>
		<th></th>
		<th>𝐻<sub>0</sub> TRUE: 𝜇 = 𝜇<sub>0</sub></th>
		<th>𝐻<sub>0</sub> FALSE: 𝜇 = 𝜇<sub>alt</sub></th>
	</tr>
	<tr>
		<th>Decision = accept <br> 'fail to reject'</th>
		<td style="background-color: #a4f4a1;">1 - 𝛼</td>
		<td style="background-color: #ffb2b2;">𝛽 <br> Type II error</td>
	</tr>
	<tr>
		<th>Decision = reject</th>
		<td style="background-color: #ffb2b2;">𝛼 <br> Type I error</td>
		<td style="background-color: #a4f4a1;">1 - 𝛽 <br> Power</td>
	</tr>
	<caption>The possible realities (null is true or null is false)
and the possible decisions (accept or reject null) we can take based
on our observed t-value</caption>
</table>

$acco$
𝛼 / Type I error
- possibility of rejecting H~0~ when it is _true_
- usually fixed a priori at **0.05**

$info$
if we were to stipulate that the
type i error be *0.005*, then the critical t-value would have had to
be set at 2.871.
$/info$
$/acco$

$acco$
𝛽 / Type I error
- possibility of accepting H~0~ when it is _false_
- conventionally recommended to be kept at **0.20 or lower**
$/acco$

$acco$
1 - 𝛽 / statistical power
- the probability of correctly rejecting H~0~ when it is *false*
- should be **larger than 0.8**
$/acco$

#### Computing power

computing power
- determining the proportion of times that the absolute observed t-value is larger than 2

```r
## power: the proportion of cases where
## we reject the null hypothesis correctly:
(pow <- mean(ifelse(abs(drep / se) > 2, 1, 0)))
```

#### The trade-off between Type I and Type II error

$gallery$
**Type I, II error**

- SE: 1
- H~0~: $\hat{\mu}$ = 0 (=> observed t-value = sample mean)
- $\mu$ = 2 (true $\mu$ is also the t-value)

![Image](img$itxx)

$widec$
<span style="background-color: #4D4D4D; color: #fff">Type I error</span> 
<span style="background-color: #CCCCCC; color: #111;">Type II error</span> 
$/widec$

- To understand this figure, one has to consider two distributions
side by side.
- First, consider the null hypothesis distribution, centered at 0. Under the null hypothesis distribution, the rejection
region lies below <span style="background-color: #4D4D4D; color: #fff">the dark colored tails of the distribution</span>. The
area under the curve in these dark-colored tails is the Type I error 
(conventionally set at 0.05) that we decide on even before we conduct the experiment and collect the data. Because the Type I error
is set at 0.05, and because the t-distribution is symmetric, the area
under the curve in each tail is 0.025. The absolute critical t-value
helps us demarcate the boundaries of the rejection regions through
the vertical lines shown in the figure.
- These vertical lines play a
crucial role in helping us understand Type II error and power. This
becomes clear when we consider the distribution representing the
alternative possible value of 𝜇, the distribution centred around 2.
In this second distribution, consider now <span style="background-color: #CCCCCC; color: #111;">the area under the curve
between the vertical lines demarcating the rejection region under
the null</span>. This area under the curve is the probability of _accepting_
the null hypothesis when the null hypothesis is _false_ with some
specific value (here, when 𝜇 has value 2).

Some interesting observations follow. Suppose that the true mean
is in fact 𝜇 = 2, as in the above illustration. Then,

- Simply _decreasing Type I error_ to a smaller value like 0.005
will _increase_ Type II error, which means that power (1-Type II
error) will fall.
- _Increasing sample size_ will squeeze the vertical lines closer to
each other because standard error will go down with increasing sample size. This will _reduce Type II error_, and therefore
_increase power_. Decreasing sample size will have the opposite
effect.
- If we design an experiment with a larger effect size, e.g., by
setting up a stronger manipulation (concrete examples will be
discussed in this book later on), our Type II error will go down,
and therefore power will go up. The figure below shows a graphical
visualization of a situation where the true mean is 𝜇 = 4. Here,
Type II error is much smaller compared to Figure 2.13, where
𝜇 = 2.

![Image](img$xuts)
$/gallery$

$warn$
In practice, researchers only rarely consider the power properties of their experiment design; the focus
is almost exclusively on Type I error. The neglect of power in
experiment design has had interesting consequences for theory development.
$/warn$

### How to compute power for the one-sample t-test

### The p-value

p-value
- the probability of obtaining the observed t-value we obtained (or a more extreme value)
- conditional on the assumption that the null hypothesis is true

computing the t-value
- calculating the area under the curve that lies beyond the absolute observed t-value on either side
- $see $down

$acco$
1. Get t-value

```r
(abs_t_value <- abs(t.test(y, mu = 450)$statistic))
```

$widec$
$down
$/widec$

```
## t
## 2.498
```
$/acco$

$acco$
2. convert t-value to p-value
```r
2 * pt(abs_t_value, df = n - 1, lower.tail = false)
```

$widec$
$down
$/widec$

```
## t
## 0.03396
```
$/acco$

$info$
The area from _both_ sides of the tail is taken because it is conventional to do what is called a two-sided t-test: our null hypothesis
is that 𝜇 = 450, and our alternative hypothesis is two-sided: 𝜇 is
either less than 450 or 𝜇 is larger than 450. When we reject the
null hypothesis, we are accepting this alternative, that 𝜇 could be
some value other than 450. Notice that this alternative hypothesis
is remarkably vague; we would reject the null hypothesis regardless of whether the sample mean turns out to be 600 or −600,
for example.

The practical implication is that the p-value gives us
the strength of the evidence against the null hypothesis; it doesn’t
give us evidence in favour of a specific alternative. 
$/info$

$warn$
In psychology and allied disciplines,
whenever the p-value falls below 0.05, it is common practice to
write something along the lines that “there was reliable evidence
for the predicted effect.” This statement is incorrect! We only ever
have evidence against the null.
$/warn$

### The distribution of the p-value under the null hypothesis

$gallery$
**The distribution of the p-value under repeated sampling, given
H~0~ = TRUE**

![Image](img$zfbe)

- the p-value has a uniform distribution _when the null hypothesis is true_
	- every value between 0 and 1 is equally likely
$/gallery$

practical implication
- when we do a single experiment and obtain a p-value (under the assumption that H~0~ is true -- and H~0~ *is* true), we are just using a random number generator to make a decision that the effect is present or absent

### Type M and S error in face of low power

|Type S error / Sign error|Type M error / magnitude error|
|---|---|
|the probability that the **sign** of the effect is incorrect|the expectation of the ratio of the absolute magnitude of the effect to the hypothesised true effect size<br>$down<br>**exaggerated result**|
|/!\ when the result is statistically significant||

$acco$
computing Type S error
- the proportion of significant cases with the wrong sign ('sign error')
- $see $down
$code$
```r
## Type S error rate | signif:
(types_sig <- mean(drep[signif] < 0))
```

$widec$
$down
$/widec$

```
## [1] 0.1754
```
$/code$
$/acco$

$acco$
computing Type M error
- the ratio by which the true effect is exaggerated

$code$
```r
## Type M error rate | signif:
(typem_sig <- mean(abs(drep[signif]) / D))
```

$widec$
$down
$/widec$

```
## [1] 7.351
```
$/code$
$/acco$

$info$
In this scenario, when power is approximately 6%, whenever we
get a significant effect, the probability of obtaining the wrong sign
is a whopping 18% and the effect is likely to be 7.35 times larger
than its true magnitude. The practical implication is as follows.
$/info$

$widec$
$down
$/widec$

practical implications
- _when power is low_, relying on the p-value ('statistical significance') to declare an effect as being present will be **misleading**
	- even if the result statistically significant!
- why? -> the significant effect is based on an **overestimate** of the effect
	- = Type M error, and the sign could be wrong too!

$wide$
- => statistical significance is **unreliable** when statistical power is low!
$/wide$

$gallery$
**Funnel plot**

![Image](img$lw60)

- true effect = 15
- repeated samples of an effect are shown under different values of power
- significant effects are <span style="color: #C7C7C7; background-color: #000">grey</span>
- observations:
	- at low power, the realistic effects are not significant
	- as power goes up, the estimates start centring around the true value (with significance)
$/gallery$

$info$
**What to do if you cannot increase power?**

1. simply report estimates with **confidence intervals** and not make binary decisions like “effect present/absent”,
2. openly acknowledge the **power limitation**
3. attempt to conduct a **direct replication** of the effect to establish robustness, and
4. attempt to **synthesise the evidence from existing knowledge**
$/info$

### Searching for significance

'searching for significance'
- attempting to get a significant result by 'cheating' the system
- $see $down

$wide$
- The experimenter gathers 𝑛 data points, then checks for significance (is 𝑝 < 0.05 or not?).
- If the result is not significant, they get more data (say, 𝑛 more
data points). Then they check for significance again.
$/wide$

## The two-sample t-test <-> the paired t-test

### Two-sample t-test

two vectors of data
- we want to know whether the two series of data are significantly different from each other

$eq$
**T-value calculation for two-sample t-test**

$$
t=\frac{d-(\mu_1 - \mu_2)}{SE} = \frac{d-0}{SE} 
$$

$widec$
𝑑: the difference between the two sample means  
µ~0~ / µ~1~: the same (H~0~ = the two groups are the same)  
𝑆𝐸: the estimated standard error of the sampling distribution of the _difference between the means_
$/widec$
$/eq$

$eq$
**Standard Error calculation**

$$
SE_\delta 
= \sqrt{\frac{\hat\sigma_1^2}{n_1} + \frac{\hat\sigma_2^2}{n_2}}
$$

$widec$
$\hat\sigma_1$ / $\hat\sigma_2$: estimates of the standard deviations for both groups  
n~1~ / n~2~: the sample sizes of both groups
$/widec$
$/eq$

$code$
**Doing the t-test**

```r
n_m <- n_f <- 19
## difference of sample means:
d <- mean(F1data$female) - mean(F1data$male)
(SE <- sqrt(var(F1data$male) / n_m + var(F1data$female) / n_f))
```

$widec$
$down
$/widec$

```r
(observed_t <- (d - 0) / SE)
```

```
## [1] 1.536
```

$widec$
$down
$/widec$

```r
## p-value:
2 * (1 - pt(observed_t, df = n_m + n_f - 2))
```

```
## [1] 0.1334
```
$/code$

#### Paired t-test

paired data
- every measurement in first group has an associated measurement in the second group

how to do a paired t-test
- 1. we subtract the vector row-wise, and get a new vector 𝑑
  2. we just do the familiar one-sample test we saw
earlier

$code$
<table>
<tr>
<th>Manual paired t-test</th>
<th>Automatic paired t-test</th>
</tr>
<td>

```r
d <- F1data$female - F1data$male
res<-t.test(d)
summary_ttest(res)
```

</td>
<td>

```r
res<-t.test(F1data$female, F1data$male, paired = TRUE)
summary_ttest(res)
```
</td>
</tr>
<tr>
<td colspan="2">

$down  

```
## [1] "t(18)=6.11 p=0"
## [1] "est.: 93.74 [61.48,125.99] ms"
```
</td>
</tr>
</table>
$/code$

$info$
Generally, one should ensure that the order in which one enters the
vectors leads to an estimate that has an easy-to-interpret sign. For
example, if object relatives (ORs) are expected to take longer to
read than subject relatives (SRs), it would be better to place the
vectors of reading times in the order OR,SR.
$/info$

$warn$
A crucial assumption in the above paired t-test is that **all the
paired measurements are independent of each other**.
Whether this assumption is correct or not (or approximately correct) depends on domain knowledge.
$/warn$

## Using paired t-tests in complex factorial designs

### Analysing a 2 x 2 repeated measures design using paired t-tests

### A complication with multiple t-tests: Inflation of Type I error probability

### Analysing a 2 × 2 × 2 repeated measures design using paired t-tests

## Common mistakes involving the (paired) t-test

### Ignoring the independence assumption

independence assumption
- paired t-test assumes that each pair of data point is independent of other pairs of data points
- assumption is that in each condition, each row is independent of each other

|condition a|condition b|subject|item|
|---|---|---|---|
|391|339|1|1|
|400|320|1|2|

$wide$
- $result independence assumption is violated!
$/wide$

$widec$
$down
$/widec$

solution
- **aggregate** the data so that each subject / item only has *one* value for each condition

### Doing a by-subjects and by-items paired t-test is generally dangerous

aggregation of multiple data points
- glosses over the inherent variability within
- source of variance is suppressed -> possibly over-enthusiastic t-value
- to take variability into account, we must switch to the linear mixed model

### The difference between a significant and a non-significant result need not itself be significant

common error
- to
find a significant effect in one study, then find a non-significant
effect in another study that changes one variable from the first
study. The conclusion then drawn is that the variable that differs
between the two studies is “significant.”