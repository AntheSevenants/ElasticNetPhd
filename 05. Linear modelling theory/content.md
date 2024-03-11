# 05. Linear modelling theory

$public=true$

## A quick review of some basic concepts

matrix
- a rectangular array of numbers with 𝑛 rows and 𝑚 columns

$code$
**Defining a matrix in R**

```r
## a 2x2 matrix:
(m1 <- matrix(1:4, 2, 2))
```

$widec$
$down
$/widec$

```
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
```
$/code$

transpose of a matrix
- rows become columns
- columns become rows
- $X^T$ is the transpose of matrix $X$

$code$
**Transposing a matrix in R**

```r
## transpose of a matrix:
t(m1)
```

$widec$
$down
$/widec$

```
##      [,1] [,2]
## [1,]    1    2
## [2,]    3    4
```
$/code$

### Matrix addition, subtraction and multiplication

$acco$
matrix addition
- cell-wise addition operation

$code$
```r
m1 + m1
```

$widec$
$down
$/widec$

```
##      [,1] [,2]
## [1,]    2    6
## [2,]    4    8
```
$/code$
$/acco$

$acco$
matrix subtraction
- cell-wise subtraction operation

$code$
```r
m1 - m1
```

$widec$
$down
$/widec$

```
##      [,1] [,2]
## [1,]    0    0
## [2,]    0    0
```
$/code$
$/acco$

$acco$
matrix multiplication
- master ai taught you how to do this :)

$code$
```r
t(matrix(m2)) %*% matrix(m2)
```
$/code$
$/acco$

$acco$
scalar matrix multiplication
- multiplying the scalar with each cell
of the matrix

```r
5 * m2
```

$widec$
$down
$/widec$

```
## [1] -0.8825 -4.9961 -6.2324 -3.7617 -0.3748
```
$/acco$

### Diagonal matrix and identity matrix

$acco$
diagonal matrix
- a square matrix that has zeroes in its off-diagonals

$code$
```r
diag(c(1, 2, 4))
```

$widec$
$down
$/widec$

```
##      [,1] [,2] [,3]
## [1,]    1    0    0
## [2,]    0    2    0
## [3,]    0    0    4
```
$/code$
$/acco$

$acco$
identity matrix
- a diagonal matrix with 1's along its diagonal
- e.g. $I_n$ (where $n$ = matrix dimensions)

$code$
```r
diag(rep(1, 3))
```

$widec$
$down
$/widec$

```
##      [,1] [,2] [,3]
## [1,]    1    0    0
## [2,]    0    1    0
## [3,]    0    0    1
```
$/code$
$/acco$

$info$
Multiplying an identity matrix with any (conformable) matrix gives that matrix back.
$/info$

### Powers of matrices

regular matrix
- $AA$ = $A^2$

diagonal matrix
- $AA$ = diagonal matrix, with the diagonal elements of A, squared

$code$
```r
m <- diag(c(1, 2, 3))
m %*% m
```

$widec$
$down
$/widec$

```
##      [,1] [,2] [,3]
## [1,]    1    0    0
## [2,]    0    4    0
## [3,]    0    0    9
```

$widec$
For all positive integers $m$, $I_n^m = I_n$.
$/widec$
$/code$

### Inverse of a matrix

$eq$
**Requirements for the inverse of a matrix**

- $A$, $B$ are square matrices ($n \times n$)
- the following relation is satisfied: $AB = BA= I_n$

$widec$
$down
$/widec$

- $result $B$ is the **inverse** of $A$ -> $B=A^{-1}$

$info$
We can do matrix division in this way, but I don't understand it, so I'm skipping it for now.
$/info$
$/eq$

$info$
If a matrix is not invertible, we say that it is **singular**.
$/info$

### Linear independence, and rank

$acco$
$deflist$
**Linear dependence**

consider a $\\3\times 3$ matrix.
the rows $r_1, r_2, r_3$ are linearly **dependent** if 
$\alpha, \beta, \gamma$, not all zero, exist such that 
$\alpha r_1+ \beta r_2+ \gamma r_3 = (0,0,0)$.
$/deflist$

linear dependence and singularity
- of the rows or columns of a matrix are linearly dependent, then the matrix is **singular** (= not invertible)

$code$
```r
(m7 <- matrix(c(1, 1, 3, 1, -1, 1, -2, 2, 4), 3, 3, byrow = TRUE))
##      [,1] [,2] [,3]
## [1,]    1    1    3
## [2,]    1   -1    1
## [3,]   -2    2    4
```

$widec$
$down
$/widec$

```
##      [,1] [,2] [,3]
## [1,]    1    1    3
## [2,]    1   -1    1
## [3,]   -2    2    4
```

---

```r
## invertible:
solve(m7)
```

$widec$
$down
$/widec$

``` 
##      [,1]    [,2]    [,3]
## [1,]  0.5 -0.1667 -0.3333
## [2,]  0.5 -0.8333 -0.1667
## [3,]  0.0  0.3333  0.1667
```
$/code$
$/acco$

$acco$
column rank of a matrix
- the maximum number of linearly independent columns in the matrix

row rank of a matrix
- the maximum number of linearly independent rows in the matrix

$result rank
- the maximum number of linearly independent columns/rows in the matrix
- row rank = column rank!
$/acco$

## The essentials of linear modelling theory

----
$widec$
Function to matrix
$/widec$
----

deterministic function $\phi(\mathbf{x},\beta)$
- input = variable value $x$, fixed value(s) $\beta$
- e.g. $y = \beta x$
- e.g. $y = \beta_0 + \beta_1 x$

$widec$
$down can be rewritten as...
$/widec$

$eq$
**Regression-as-a-matrix**

$$
\begin{aligned}
y &= \beta_0 + \beta_1 x\\
&= \beta_0\times 1 + \beta_1 x\\
&= \begin{pmatrix}
1 & x\\
\end{pmatrix}
\begin{pmatrix}
\beta_0 \\
\beta_1 \\
\end{pmatrix}
\end{aligned}
$$

$widec$
$down
$/widec$

$$
y = \phi(x,\beta)
$$
$/eq$

$info$
In a statistical model, we don't expect an equation like $y=\phi(x,\beta)$ to fit all the points exactly. For example, we could come up with an 
equation that, given a word's frequency, gives a prediction regarding that word's reaction time:

$$
\text{predicted reaction time} = \beta_0 + \beta_1 \text{frequency}
$$

$gallery$
![Image](img$t0cs)

- $result the model doesn't *perfectly* capture the relationship between frequency and reading times
$/gallery$
$/info$

$widec$
$down we need an adaption to our regression function
$/widec$

$eq$
**Non-deterministic function**

$$
y=\phi(x,\beta,\epsilon)=\beta_0+\beta_1x+\epsilon
$$

$widec$
$\varepsilon$: an error random variable  
$result has own PDF: $\epsilon \sim N(0,\sigma)$.
$/widec$
$/eq$

$eq$
**General linear model**

- a non-deterministic function

$$
Y=f(x)^T\beta +\epsilon 
$$
$/eq$

$eq$
**General linear model (matrix formulation)**

$$
\begin{aligned}
Y &= X \cdot \beta + \epsilon \\
& \Updownarrow \\
y_n &= f(x_n)^T \beta + \epsilon_n, n=1,\dots,N
\end{aligned}
$$

$info$
$f(x_n)$ is the function that outputs the $\left( \begin{matrix}1 & x_n\end{matrix} \right)$ matrices.
$/info$

$example$
**Example data**

$$
\begin{pmatrix}
Y_1 \\
Y_2\\
Y_3 \\
\end{pmatrix}
=
\begin{pmatrix}
1 & x_1 \\
1 & x_2 \\
1 & x_3 \\
\end{pmatrix}
\begin{pmatrix}
\beta_0 \\
\beta_1 \\
\end{pmatrix}+ 
\begin{pmatrix}
\epsilon_1 \\
\epsilon_2 \\
\epsilon_3
\end{pmatrix}
$$

Here, $f(x_1)^T = (1~x_1)$, and is the first row of the matrix $X$, 
$f(x_2)^T = (1~x_2)$ is the second row, and 
$f(x_3)^T = (1~x_3)$ is the third row.
$/example$

$info$
The "expectation" or mean of $Y$, written , is .
$/info$
$/eq$

expectation / mean of Y / $E[Y]$
- $X \cdot \beta$
- $todo: why? this doesn't result in a scalar?

$\beta$
- a $p \times 1$ matrix
- 𝑝 = number of parameters

design matrix / model matrix / 𝑋
- an $n \times p$ matrix
- 𝑝 = number of parameters, 𝑛 = data points

### Least squares estimation: geometric argument

#### Deterministic <-> non-deterministic models

----
$widec$
How are $\beta$ parameters are estimated?
$/widec$
----

$acco$
deterministic model $y=\phi(f(x)^t,\beta)=\beta_0+\beta_1x$
- implies a perfect fit to all data points
- "like solving an equation"
- you have 𝑥, you have 𝑦, you just need to find the operation needed to transform 𝑥 into 𝑦
$/acco$

$acco$
non-deterministic model $y=\phi(f(x)^T,\beta,\epsilon)$
- no solution possible!
- our best bet: you want to find the operation which brings the transformation of 𝑥 *as close as possible* to 𝑦
- => we want to minimise $\mid b-Ax\mid$
$/acco$

#### Estimating $\beta$

----
$widec$
Goal: finding value of $\beta$ such that observed $Y$ is as close to its expected value $X \cdot \beta$.
$/widec$
----

$info$
In order to be able to identify $\beta$ from $X\beta$, the linear transformation $\beta \rightarrow X\beta$ should be one-to-one, so that every possible value of $\beta$ gives a different $X\beta$. This in turn requires that $X$ be of full rank $p$. So, if a design matrix $X$ is $n\times p$, then it is necessary that $n\geq p$. There must be at least as many observations as parameters. If this is not true, then the model is said to be **over-parameterized**.
$/info$

$Y$
- a point in 𝑛-dimensional space

$X\beta$
- a 𝑝-dimensional subspace of this space

$wide$
- $result there will be **one point** in this subspace which is closest to $Y$ in terms of Euclidean distance
$/wide$

$gallery port$
$widec$
![Image](img$1b1u)
$/widec$

$widec$
$Y$: alle responswaarden, voorgesteld als een hoogdimensioneel punt,  
$X\beta$: alle mogelijke transformaties van $X$,  
$X\hat{\beta}$: de transformatie van $X$ die het dichtst bij $Y$ in de buurt komt,  
$\varepsilon$: de afwijking tussen $X\hat{\beta}$ en $Y$
$/widec$
$/gallery port$

$\hat{\beta}$
- the unique $\beta$ that corresponds to the closest point to $Y$
- => least squares estimator of $\beta$

$info$
Notice that $\epsilon=(Y - X\hat\beta)$ and $X\beta$ are perpendicular to each other. Because the dot product of two perpendicular (orthogonal) vectors is 0, we get the result:

$$
(Y- X\hat\beta)^T X \beta = 0 \Leftrightarrow (Y- X\hat\beta)^T X = 0 
$$
$/info$

### The expectation and variance of the parameters beta

$eq$
**Our current model**

$$
Y = X\beta + \epsilon \quad  \epsilon\sim N(0,\sigma)
$$

$widec$
and $Y$ are independent and identically distributed (= *iid* assumption)
$/widec$
$/eq$

$result consequences
- $E[\epsilon]=0$
- $\operatorname{Var}(\epsilon)=\sigma^2 I_n$
- $E[Y]=X\beta=\mu$
- $\operatorname{Var}(Y)=\sigma^2 I_n$
- $\operatorname{Var}(\hat\beta)= \sigma^2 (X^TX)^{-1}$

### Hypothesis testing using Analysis of variance (ANOVA)

#### Using ANOVA

$code$
**Comparing two models (one nested inside another)**

```r
m1 <- lm(y ~ x_c)
m0 <- lm(y ~ 1)
anova(m0, m1)
```

$widec$
$down
$/widec$

```
## Analysis of Variance Table
## 
## Model 1: y ~ 1
## Model 2: y ~ x_c
##   Res.Df  RSS Df Sum of Sq    F Pr(>F)    
## 1   1658 96.7                             
## 2   1657 91.8  1      4.95 89.5 <2e-16 ***
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
$/code$

$info$
The F-score you get here is actually the square of the t-value you
get in the linear model summary. This is because $t^2 = F$.
$/info$

#### How does ANOVA work?

$eq$
**Residual**

$$
e = Y - X\hat\beta
$$
$/eq$

$wide$
$todo: ik versta het niet
$/wide$

### Some further important topics in linear modelling

#### The variance inflation factor: checking for multicollinearity

multiple predictors
- work just fine in linear models
- e.g. $see $down

$code$
```r
m <- lm(RT ~ Frequency + FamilySize, lexdec)
summary(m)
```

$widec$
$down
$/widec$

```
## 
## Call:
## lm(formula = RT ~ Frequency + FamilySize, data = lexdec)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.5510 -0.1608 -0.0344  0.1204  1.0969 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  6.56385    0.02683  244.68  < 2e-16 ***
## Frequency   -0.03531    0.00641   -5.51  4.1e-08 ***
## FamilySize  -0.01565    0.00938   -1.67    0.095 .  
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.235 on 1656 degrees of freedom
## Multiple R-squared:  0.0528, Adjusted R-squared:  0.0517 
## F-statistic: 46.2 on 2 and 1656 DF,  p-value: <2e-16
```
$/code$

multicolinearity
- problem when multiple predictors are highly correlated
- estimation equation becomes ill-conditioned -> small changes in the data can cause large changes in $\beta$
- also, some of the elements of $\sigma^2 (X^TX)^{-1}$ will be large--standard errors and covariances can be large.

----

Variance Inflation Factor / VIF
- can help with checking for multicolinearity

$eq$
**Variance Inflation Factor for one predictor 𝑗**

$$
\operatorname{VIF}_j = \frac{1}{1-R_j^2}
$$

$widec$
$R_j^2$: coefficient of determination for predictor 𝑗  
$result obtained by regressing the j-th explanatory variable against all the 
other explanatory variables
$/widec$

- if predictor 𝑗 is uncorrelated with all other predictors, then $VIF_j=1$,
- if predictor 𝑗 is highly correlated with (some of) other predictors, the VIF will be high
$/eq$

$code$
**Computing the variance inflation factor in R**

```r
library(car)
m <- lm(RT ~ Frequency + FamilySize, lexdec)
vif(m)
```

$widec$
$down
$/widec$

```
##  Frequency FamilySize 
##          2          2
```

If the predictors are uncorrelated, VIF will be near 1 in each case.
Dobson et al mention that VIF of greater than 5 is cause for worry.
$/code$

#### Checking model assumptions

$acco$
**1.** checking the **distribution** of the residuals
- residuals *should* be normally distributed
- normality assumption is a crucial part of the assumptions of the hypothesis test!
- => qq plot

$code$
```r
library(car)
qqplot(residuals(m))
```

$widec$
$down
$/widec$

$gallery$
![image](img$btxo)
$/gallery$

$info$
kolmogorov-smirnov and
shapiro-wilk are formal tests of normality and are only useful for
large samples; they not very powerful and not much better than
diagnostic plots like the qq-plot shown above.
$/info$
$/code$
$/acco$

$acco$
**2.** checking the **independence** of the residuals
- index-plots or correlation between pairs of residuals
- $todo ik begrijp dit niet

$code$
```r
acf(residuals(m))
```

$widec$
$down
$/widec$

$gallery$
![Image](img$53s0)

In our model (which is the multiple regression we did in connection
with the collinearity issue), we have a serious violation of independence. This is because in this model we are not taking into account
the fact that we have repeated measures. The repeated measures
create a dependency in the residuals, violating the independence
assumption. This problem can be largely solved by fitting a linear
mixed model.
$/gallery$
$/code$
$/acco$

$acco$
**3.** checking the homoscedasticity  (= equality of variance)
- plot residuals against fitted values
- fanning-out => violation
- quadratic trend in a plot of residuals against predictor x could suggest that a quadratic predictor term is needed
- /!\ we will never have a perfect straight line in such a plot

$code$
```r
op <- par(mfrow = c(2, 2), pty = "s")
plot(m)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$h5o1)
$/gallery$
$/code$
$/acco$

### Generalised linear models

#### Introduction: logistic regression

##### Beetle dataset

Beelte dataset
- shows the number of beetles killed after five
hours’ exposure to gaseous carbon disulphide at various concentrations

[ Beelte dataset ]
|dose|number|killed|
|---|---|---|
|1.69|59|6|
|1.72|60|13|
|1.76|62|18|
|1.78|56|28|
|1.81|63|52|
|1.84|59|53|
|1.86|62|61|
|1.88|60|60|

research question
- does dose affect probability of killing
insects?

$code$
**Computing proportions**

```r
(beetle$propn.dead <- beetle$killed / beetle$number)
```

$widec$
$down
$/widec$

```
## [1] 0.1017 0.2167 0.2903 0.5000 0.8254 0.8983 0.9839
## [8] 1.0000
```
$/code$

$gallery$
**Relationship between dose and proportion**

![Image](img$3vwq)

$widec$
Notice that the y-axis is by definition bounded between 0 and 1.
$/widec$
$/gallery$

##### Linear regression?

$code$
**Fitting a linear regression model**

- the predictor is centred, because of $see $up

```r
fm <- lm(propn.dead ~ scale(dose, scale = FALSE), beetle)
summary(fm)$coefficients
```

$widec$
$down
$/widec$

```
##                            Estimate Std. Error t value  Pr(>|t|)
## (Intercept)                   0.602    0.03065   19.64 1.129e-06
## scale(dose, scale = FALSE)    5.325    0.48573   10.96 3.422e-05
```

$gallery$
**Model with regression line**

![Image](img$6bs9)
$/gallery$
$/code$

$result issues
- intercept represents mean proportion of beetles killed (OK)
- slope represents increase in the proportion of beetles killed when the dose is increased by one unit (from initial dose of 0)
	- problem: this proportion is larger than 1 -> impossible for a proportion

##### Log odds

~~proportion~~
- **log odds**
- the ratio of: probability of succes / probability of failure
- then: we take the log

$widec$
$down
$/widec$

$eq$
**Regression formula: log odds**

$$
\log \frac{p}{1-p} = \beta_0 + \beta_1 \text{dose}
$$


- $result logistic regression model
$/eq$

$eq$
**Going back to the probability space**

$$
\begin{aligned}
\log \frac{p}{1-p} &= \beta_0 + \beta_1 \text{dose} \\
\exp \log \frac{p}{1-p} &= \\
 \frac{p}{1-p} &= \exp( \beta_0 + \beta_1 \text{dose}) \\
p &= \frac{\exp( \beta_0 + \beta_1 \text{dose})}{1+\exp( \beta_0 + \beta_1 \text{dose})}

\end{aligned}
$$
$/eq$

##### Fitting a logistic regression model

$code$
```r
fm1 <- glm(propn.dead ~ dose,
  family = binomial(logit),
  weights = number,
  data = beetle
)
summary(fm1)
```

$info$
Note that as long as we are willing
to avoid interpreting the intercept and just interpret the estimate
of 𝛽1, there is no need to center the predictor here.
$/info$

$widec$
$down
$/widec$

```
## 
## Call:
## glm(formula = propn.dead ~ dose, family = binomial(logit), data = beetle, 
##     weights = number)
## 
## Deviance Residuals: 
##    Min      1Q  Median      3Q     Max  
## -1.594  -0.394   0.833   1.259   1.594  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -60.72       5.18   -11.7   <2e-16 ***
## dose           34.27       2.91    11.8   <2e-16 ***
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 284.202  on 7  degrees of freedom
## Residual deviance:  11.232  on 6  degrees of freedom
## AIC: 41.43
## 
## Number of Fisher Scoring iterations: 4
```
$/code$

$gallery$
**Plotting the observed proportions**

![Image](img$lete)
$/gallery$

$code$
**Using the regression model for prediction**

```r
## compute log odds of death for
## concentration 1.7552:
x <- as.matrix(c(1, 1.7552))
# log odds:
(log.odds <- t(x) %*% coef(fm1))
```

$widec$
$down
$/widec$

```
##         [,1]
## [1,] -0.5662
```
$/code$

#### Multiple logistic regression

[ Hindi dataset ]
|subj|expt|item|word_complex|SC|skip|
|---|---|---|---|---|---|
|  10|hnd1|   6|         0.0| 1|   1|
|  10|hnd1|   6|         0.0| 1|   1|
|  10|hnd1|   6|         0.0| 2|   0|
|  10|hnd1|   6|         1.5| 1|   1|
|  10|hnd1|   6|         0.0| 1|   1|
|  10|hnd1|   6|         0.5| 1|   0|

$widec$
notice that `skip` is a binary variable, not a proportion!
$/widec$

response variable
- skipping probability -> probability of skipping a word entirely (never fixating on it)

$code$
```r
fm_skip <- glm(skip ~ word_complex + SC, family = binomial(), hindi10)
summary(fm_skip)
```

$widec$
$down
$/widec$

```
## 
## Call:
## glm(formula = skip ~ word_complex + SC, family = binomial(), 
##     data = hindi10)
## 
## Deviance Residuals: 
##    Min      1Q  Median      3Q     Max  
## -1.100  -0.884  -0.674   1.256   2.682  
## 
## Coefficients:
##              Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -0.1838     0.0266   -6.91  4.7e-12 ***
## word_complex  -0.6291     0.0281  -22.38  < 2e-16 ***
## SC            -0.5538     0.0224  -24.70  < 2e-16 ***
## ---
## Signif. codes:  
## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 31753  on 27065  degrees of freedom
## Residual deviance: 30492  on 27063  degrees of freedom
## AIC: 30498
## 
## Number of Fisher Scoring iterations: 4
```
$/code$

#### Deviance

$eq$
**Deviance**

$$
D = 2[logLik(\bar{x}; y) - logLik(mu_0; y)]
$$

$widec$
$\\logLik(\bar{x}; y)$: the log likelihood of the saturated model (the model with the maximal number of parameters that can be fit),  
$\\logLik(mu_0; y)$: the log likelihood of the model with parameter of interest having the null hypothesis value $\mu_0$
$/widec$

$info$
As we
saw earlier, D has a chi-squared distribution.
$/info$
$/eq$

$eq$
**Deviance for the normal distribution**

$$
D = \frac{1}{\sigma^2}\sum (y_i - \bar{y})^2
$$
$/eq$

idea of deviance
- if the model fit is good, Deviance will have a $\chi^2$ distribution with $N-p$ degrees of freedom
	- $N$ = the number of data-points, $p$ = the number of parameters

residual deviance
- the difference in deviance between two models
- has a $\chi^2$ distribution with dfs being $p-q$
	- $q$ = the number of parameters in the null model, $p$ = the number of parameters in the full model