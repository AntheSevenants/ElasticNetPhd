# 04. Hypothesis testing using the likelihood ratio test

$public=true$

## The likelihood ratio test: the theory

### Example distribution

our distribution 𝑋
- 𝑋~1~, ..., 𝑋~𝑛~ independent data
- normally distributed
- mean: 𝜇
- standard deviation: 𝜎

hypothesis to test
- 𝐻~0~ ∶ 𝜇 = 𝜇~0~
- alternative: 𝐻~1~ ∶ 𝜇 != 𝜇~0~

$widec$
$down example
$/widec$

example data
- 10 data points generated from 𝑁𝑜𝑟𝑚𝑎𝑙(0, 1) ($see $down)

$code$
```r
y <- rnorm(10)
```
$/code$

### Computing the likelihood of our example data

$code$
**Joint likelihood under the null hypothesis (𝜇 = 0)**

```r
likNULL <- dnorm(y, mean = 0, sd = 1) %>% prod()
likNULL
```

$widec$
$down
$/widec$

```
## [1] 9.151e-06
```

$info$
The function `dnorm` only returns the value of the probability density function, _not_ the probability itself. How we can turn this into a *likelihood* then is a very good question ($todo).
$/info$
$/code$

$code$
**Joint likelihood under the null hypothesis (𝜇 = 0), log scale**

```r
loglikNULL <- dnorm(y, mean = 0, sd = 1, log = TRUE) %>% sum()
loglikNULL
```

$widec$
$down
$/widec$

```
## [1] -11.6
```
$/code$

$code$
**Joint likelihood with 𝜇 = sample mean, log scale**

```r
loglikALT <- dnorm(y, mean = mean(y), sd = 1, log = TRUE) %>% sum()
loglikALT
```

$widec$
$down
$/widec$

```
## [1] -11.59
```
$/code$

$widec$
$todo maybe rework this using a table
$/widec$

### Definition of the test

log likelihood ratio test
- compares the **ratio of likelihoods of two models**, on the log scale
	- *difference* of likelihood between two models (log rules)
- test chooses the model with the higher log likelihood (given that this likelihood is "high enough")

$eq$
$$
\Lambda = \frac{max_{\theta\in \omega_0}(lik(\theta))}{max_{\theta\in \omega_1}(lik(\theta))}
$$

$widec$
𝜃: the parameter which has an influence on the likelihood,  
𝜔~0~ = {𝜇~0~},  
𝜔~1~ = {∀𝜇 ∣ 𝜇 != 𝜇~0~},  
$\max$: selects the maximum value of choices of parameter values  
$result for 𝜇~0~: only one value  
$result for alternative: MLE $\hat{\theta}$ is the maximum value
$/widec$

$info$
We can evaluate the likelihood at 𝜇~0~ (the null hypothesis value of the parameter)
and evaluate the likelihood using the maximum likelihood estimate
$\hat{\theta}$ of the parameter, as we did above.
$/info$
$/eq$

$eq$
**Verbose log likelihood ratio using normally distributed data**

$$
\begin{aligned}
\Lambda &= \frac{lik(\theta=\mu_0)}{lik(\theta=\bar{X})} \\
		&=
\frac{\frac{1}{(\sigma\sqrt{2\pi})^n} 
           exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (X_i - \mu_0)^2  \right)}{\frac{1}{(\sigma\sqrt{2\pi})^n} 
           exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (X_i - \bar{X})^2  \right)} \\
		&= 
\frac{exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (X_i - \mu_0)^2  \right)}{
        exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (X_i - \bar{X})^2  \right)}
\end{aligned} 
$$

$widec$
$down (a lot of rearranging and rewriting later)
$/widec$

$$
-2 \log \Lambda =    \frac{(\bar{X} - \mu_0)^2 }{\frac{\sigma^2}{n}}
$$

$wide$
- $result very reminiscent of the t-test (= this is the square of the t-test formula)
- $result comes down to: we reject the null when the distance between $\bar{X}$ and $\mu_0$ (normalized by the square of the standard error) is large
- => we reject the null hypothesis when negative two times the difference in log
likelihood, is large
$/wide$
$/eq$

### When is $-2 \log \Lambda$ large?

statistical theory
- for large *n*, the distribution of $-2 \log \Lambda$ approaches the chi-squared distribution
- 𝑑𝑓 ~ the difference in the number of parameters between the two models being compared

$widec$
$down
$/widec$

$eq$
**Likelihood ratio test statistic**

$$
\begin{aligned}
LRT =& -2\times \frac{Lik(\theta_0)}{Lik(\theta_1)} \\
\log LRT =& -2\times \{logLik(\theta_0)-logLik(\theta_1)\}\\
\end{aligned}
$$

$widec$
𝐿𝑅𝑇: likelihood ratio test statistic,  
𝐿𝑖𝑘(𝜃): the likelihood given some value 𝜃 for the parameter,  
𝑙𝑜𝑔𝐿𝑖𝑘(𝜃): the **log** likelihood given some value 𝜃 for the parameter,  
𝜃~0~: estimate of 𝜃 under the null hypothesis,  
𝜃~1~: estimate of 𝜃 under the alternative hypothesis
$/widec$

$widec$
The likelihood ratio test rejects 𝐻~0~ if log 𝐿𝑅𝑇 is sufficiently large
$/widec$
$/eq$

$eq$
**Likelihood ratio test statistic distribution property**

$$
\log LRT \rightarrow \chi_r^2  \text{ as }  n \rightarrow \infty
$$

$widec$
𝑛: sample size,  
𝑟: degrees of freedom -> difference in the number of parameters under the null and alternative hypotheses
$/widec$

$wide$
- $result As the sample size approaches infinity, log 𝐿𝑅𝑇 approaches the chi-squared distribution.
$/wide$
$/eq$

## A practical example using simulated data

$code$
**Simulated linear model data**

```r
x <- 1:10
y <- 10 + 20 * x + rnorm(10, sd = 10)
```

- $result we generate a **linear model**
- $result slope = 20
$/code$

defining the null hypothesis
- we will test whether slope == 0
- we know this is untrue ($see $up) => slope = 20

|null hypothesis model|alternative hypothesis model|
|---|---|
|`m0 <- lm(y ~ 1)`|`m1 <- lm(y ~ x)`|
|model without a slope|model with estimate of a slope|
|1 parameter|2 parameters|

$widec$
$down
$/widec$

$code$
**Computing the likelihood ratio statistic**

```r
LogLRT <- -2 * (logLik(m0) - logLik(m1))
## observed value:
LogLRT[1]
```

$widec$
$down
$/widec$

```
## [1] 34.49
```
$/code$

difference in the number of parameters
- $\\2 - 1 = 1$
- => log 𝐿𝑅𝑇 has distribution $\chi_1^2$

$widec$
$down Is the observed value $\\34.49$ unexpected under this distribution?
$/widec$

calculating the probability
- = probability of $\\34.49$ or a more extreme value
- (given the $\chi_1^2$ distribution)

$code$
**Computing the probability**

```r
pchisq(LogLRT[1], df = 1, lower.tail = FALSE)
```

$widec$
$down
$/widec$

```
## [1] 4.286e-09
```

$info$
Just like the t-test has a criticual t-value, the chi-squared distribution has the critical value:

```r
## critical value:
qchisq(0.95, df = 1)
```

$widec$
$down
$/widec$

```
## [1] 3.841
```

If minus two times the observed difference in log likelihood is larger
than this critical value, we reject the null hypothesis.
$/info$
$/code$

$info$
Note that in the likelihood test above, we are comparing one nested
model against another: the null hypothesis model is nested inside
the alternative hypothesis model. What this means is that the
alternative hypothesis model contains all the parameters in the
null hypothesis model (i.e., the intercept) plus another one (the
slope).
$/info$

## A real-life example: The English relative clause data

### Likelihood ratio test and the linear mixed model

----
$widec$
We again use the reading times data
$/widec$
----

null hypothesis
- slope parameter
- sum contrast coding
	0. $\beta_0$ = grand mean
	1. $\beta_1$ = the amount by which subject and
object relative clause mean reading times deviate from the grand
mean
- hypothesis: $\beta_1$ = 0 ~= any difference in means between the two relative
clause types?

[ Sum coding, +1 = OR, -1 = SR ]
|object relatives|subject relatives|
|---|---|
|$\mu_{or}=\beta_0 + \beta_1$|$\mu_{sr}=\beta_0 - \beta_1$|

$eq$
**Testing the null hypothesis**

$$
\begin{aligned}
H_0 : (\beta_0 + \beta_1)-(\beta_0 - \beta_1) &= 0 \\
\beta_0 + \beta_1-\beta_0 + \beta_1 &= 0 \\
2\beta_1 &= 0 \\
\beta_1 &= \frac{0}{2} \\
\beta_1 &= 0
\end{aligned}
$$
$/eq$

### Using real data

|null model|full model|
|---|---|
|`logrt ~ 1 + (1 + so | subject) + (1 + so || item)`|`logrt ~ 1 + so + (1 + so | subject) + (1 + so || item)`|
|no slope parameter|a slope parameter|

$code$
**Building the two models**

$widec$
/!\ we use sum coding! ($see $up)
$/widec$

```r
library(lingpsych)
data("df_gg05e1")
gg05e1 <- df_gg05e1
gg05e1$so <- ifelse(gg05e1$condition == "objgap", 1, -1)
gg05e1$logrt <- log(gg05e1$rawRT)
library(lme4)
m0 <- lmer(logrt ~ 1 + (1 + so | subject) + (1 + so || item), gg05e1)
m1 <- lmer(logrt ~ 1 + so + (1 + so | subject) + (1 + so || item), gg05e1)
```

$info$
Notice that **we keep all random effects in the null model**. We say
that the null model is nested inside the full model.
$/info$
$/code$

$code$
**Comparing the two models' log likelihoods**

The `anova` function compares the two models' log likelihoods.

```r
anova(m0, m1)
```

$widec$
$down
$/widec$

```
## Data: gg05e1
## Models:
## m0: logrt ~ 1 + (1 + so | subject) + ((1 | item) + (0 + so | item))
## m1: logrt ~ 1 + so + (1 + so | subject) + ((1 | item) + (0 + so | item))
##    npar AIC BIC logLik deviance Chisq Df Pr(>Chisq)  
## m0    7 707 739   -347      693                      
## m1    8 703 739   -343      687  6.15  1      0.013 *
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
$/code$

### t-test or LRT?

likelihood ratio test
- the most **general** method for hypothesis testing

t-test output
- can be used, but only when the data are **balanced**
- ~~missing data~~

$info$
One can also use the likelihood ratio test to evaluate **whether a
variance component should be included or not**. For example, is
the correlation parameter justified for the subjects random effects?
Recall that we had a correlation of 0.58. Is this statistically significant? One can test this in the following way:

```r
m1 <- lmer(logrt ~ 1 + so + (1 + so | subject) + (1 + so || item), gg05e1)
m1NoCorr <- lmer(logrt ~ 1 + so + (1 + so || subject) + (1 + so || item), gg05e1)
anova(m1, m1NoCorr)
## refitting model(s) with ML (instead of REML)
```

$widec$
$down
$/widec$

```
## Data: gg05e1
## Models:
## m1NoCorr: logrt ~ 1 + so + ((1 | subject) + (0 + so | subject)) + ((1 | item) + (0 + so | item))
## m1: logrt ~ 1 + so + (1 + so | subject) + ((1 | item) + (0 + so | item))
##          npar AIC BIC logLik deviance Chisq Df Pr(>Chisq)   
## m1NoCorr    7 710 741   -348      696         
## m1          8 703 739   -343      687   8.7  1 0.0032 **
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

- $result The test indicates that we can reject the null hypothesis that the
correlation parameter is 0.
$/info$