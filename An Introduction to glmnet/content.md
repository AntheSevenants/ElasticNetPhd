$public=true$

# An Introduction to glmnet

$widec$
from [An Introduction to `glmnet`](https://glmnet.stanford.edu/articles/glmnet.html)
$/widec$

## Introduction

### About glmnet

`glmnet`
- R package
- fits generalized linear and similar models via ==_penalized_ maximum likelihood==

regularization path
- computed for lasso / elastic net penalty
- concerning regularization parameter ==lambda ($l)==

### Problem statement

$eq$
$$
\min_{\beta_0,\beta} \frac{1}{N} \sum_{i=1}^{N} w_i l(y_i,\beta_0+\beta^T x_i) + \lambda\left[(1-\alpha)\|\beta\|_2^2/2 + \alpha \|\beta\|_1\right]
$$

$widec$
over a grid of values of $l covering the entire range of possible solutions  
${l}(y_i,\eta_i)$: the negative log-likelihood contribution for observation 𝑖  
ɑ: _elastic net_ penalty (controls distribution between lasso and ridge)  
$l: controls strength of the penalty
$/widec$
$/eq$

### Differences between ridge and lasso penalty

|ridge penalty|lasso penalty|
|---|---|
|shrinks the coefficients of correlated predictors towards each other|picks one correlated predictor and discards the others|

$info$
The ==elastic net penalty mixes these two==: if predictors are correlated in groups, an $\alpha = 0.5$ tends to either _select_ or _leave out_ the entire group of features.
$/info$

### Installation

### Quick start

#### Housekeeping

$code$
**Loading the library**

```R
library(glmnet)
```
$/code$

default model
- Gaussian linear model / "least squares"

$code$
**Loading sample data**

```r
data(QuickStartExample)
x <- QuickStartExample$x
y <- QuickStartExample$y
```

$gallery port$
![Image](img$0qko)
![Image](img$u540)
$/gallery port$

The command loads an input matrix `x` and a response vector `y` from this saved R data archive.
$/code$

#### Fitting and inspection

$code$
**Fitting a basic model**

```r
fit <- glmnet(x, y)
```

- `fit`: an object of class `glmnet`
	- contains all the relevant information of the fitted model for further use

$warn$
We do not encourage users to extract the components directly. Instead, various methods are provided for the object such as `plot`, `print`, `coef` and `predict` that enable us to execute those tasks more elegantly.
$/warn$
$/code$

$code$
**Model visualisation**

```r
plot(fit, label=TRUE)
```

$gallery$
![Image](img$v0ed)

- each curve = a variable (i.e. in linguistic case: a lexical verb, or another predictor)
	- shows trajectory of coefficient against the $\ell_1$-norm of the whole coefficient vector as $\lambda$ varies
- axis above: indicates the number of nonzero coefficients at the current $\lambda$, which is the effective degrees of freedom (_df_) for the lasso
$/gallery$
$/code$

$code$
**Summary**

```r
print(fit)
```

$widec$
$down
$/widec$

```plain
## 
## Call:  glmnet(x = x, y = y) 
## 
##    Df  %Dev  Lambda
## 1   0  0.00 1.63100
## 2   2  5.53 1.48600
## 3   2 14.59 1.35400
## 4   2 22.11 1.23400
## 5   2 28.36 1.12400
## 6   2 33.54 1.02400
....
```

- summary of `glmnet` path at each step
- properties:  
	|`Df`|`%Dev`|`Lambda`|
	|---|---|---|
	|number of nonzero coefficients|percent deviance explained|value of $\lambda$|

$info$
Although `glmnet` fits the model for 100 values of lambda by default, it stops early if `%dev` does not change sufficently from one lambda to the next (typically near the end of the path.) 
$/info$
$/code$

$code$
**Obtaining model coefficients**

```r
coef(fit, s = 0.1)
```

$widec$
$down
$/widec$

```
## 21 x 1 sparse Matrix of class "dgCMatrix"
##                       s1
## (Intercept)  0.150928072
## V1           1.320597195
## V2           .          
## V3           0.675110234
## V4           .          
## V5          -0.817411518
## V6           0.521436671
## V7           0.004829335
....
```

- `s` encodes the lambda -> default = the ==entire sequence used to create the model==
	- [discussion on the 'best' lambda](https://stackoverflow.com/questions/30565457/getting-glmnet-coefficients-at-best-lambda)
$/code$

#### Inference

$code$
**Predicting**

```r
set.seed(29)
nx <- matrix(rnorm(5 * 20), 5, 20)
predict(fit, newx = nx, s = c(0.1, 0.05))
```

$widec$
$down
$/widec$

```plain
##              s1         s2
## [1,] -4.3067990 -4.5979456
## [2,] -4.1244091 -4.3447727
## [3,] -0.1133939 -0.1859237
## [4,]  3.3458748  3.5270269
## [5,] -1.2366422 -1.2772955
```
$/code$

#### Cross-validation

`glmnet`
- returns a sequence of models for the users to choose from
- however, users may prefer the _software_ to select one of them

$widec$
$down
$/widec$

cross-validation
- simplest and most widely used method for the task

$code$
**Cross validation and Elastic Net Regression**

```r
cvfit <- cv.glmnet(x, y)
```

- `cvfit`: a `cv.glmnet` object -> list with all the ingredients of the cross-validated fit

$warn$
As with `glmnet`, we do _not_ encourage users to extract the components directly, except for viewing the selected values of $\lambda$.
$/warn$
$/code$

$code$
**Plotting a `cv.glmnet` object**

```R
plot(cvfit)
```

$gallery$
![Image](img$irew)

- <span style="text-decoration: underline; text-decoration-color: red; text-decoration-style: dotted;">cross-validation curve:</span> mean square error by $\log(\lambda)$
- `lambda.min`: the value of $\lambda$ that gives minimum mean cross-validated error
- `lambda.1se`: the value of $\lambda$ that gives the most regularised model such that cross-validated error is within one standard error of the minimum

$info$
[`lambda.min` **or** `lambda.1se`**?**](https://stats.stackexchange.com/questions/138569/why-is-lambda-within-one-standard-error-from-the-minimum-is-a-recommended-valu)

> We often use the “one-standard-error” rule when selecting the best model; this ==acknowledges the fact that the risk curves are estimated with error==, so errs on the side of parsimony.

[(Friedman, Hastie, and Tibshirani, 2010)](http://www.jstatsoft.org/v33/i01/paper)
$/info$

$info$
[**Why do we not want our lambda to be too big?**](https://stats.stackexchange.com/questions/351990/why-dont-we-want-to-choose-a-big-lambda-in-ridge-regression)

> The issue is underfitting, yes. If λ
 is too big, the coefficients will be smaller than they ought to be to get the best predictive accuracy. In the most extreme case of an arbitrarily large λ
, all the coefficients are forced to be arbitrarily close to 0 regardless of the data, so the model is maximally underfit.  
  
> However, a ridge penalty will rarely change the degree of any polynomials, since it does not force any coefficients to be exactly 0.
$/info$
$/gallery$
$/code$

$code$
**Getting lambda values**

<table>
<tr>
<th>minimal error</th>
<th>minimal error + 1 standard deviation</th>
</tr>
<tr>
<td>

```r
cvfit$‍lambda.min
```

$widec$
$down
$/widec$

```
## [1] 0.06284188
```

</td>
<td>

```r
cvfit$‍lambda.1se
```

$widec$
$down
$/widec$

```
## [1] 0.1322761
```
</td>
</tr>
</table>

$info$
You can plug these values into other functions such as `coef` and `predict` as well.

```r
coef(cvfit, s = "lambda.min")
```

$widec$
$down
$/widec$

```
## 21 x 1 sparse Matrix of class "dgCMatrix"
##                       s1
## (Intercept)  0.145832036
## V1           1.340981414
## V2           .          
## V3           0.708347140
## V4           .          
## V5          -0.848087765
## V6           0.554823782
## V7           0.038519738
....
```

```r
predict(cvfit, newx = x[1:5,], s = "lambda.min")
```

$widec$
$down
$/widec$

```
##      lambda.min
## [1,] -1.3574653
## [2,]  2.5776672
## [3,]  0.5846421
## [4,]  2.0280562
## [5,]  1.5780633
....
```
$/info$

$info$
`lambda.1se` is the default when paired with a `cv.glmnet` object.
$/info$
$/code$

## Linear Regression: `family = "gaussian"`

### Formal definition

`"gaussian"`
- the default `family` argument for the function `glmnet`

$eq$
$$
\begin{aligned}
\min_{(\beta_0, \beta) \in \mathbb{R}^{p+1}}\frac{1}{2N} \sum_{i=1}^N & (y_i -\beta_0-x_i^T \beta)^2+ \\
& \lambda \left[ (1-\alpha)\|\beta\|_2^2/2 +  \alpha\|\beta\|_1\right]
\end{aligned}
$$

$widec$
observations: $x_i \in \mathbb{R}^p$,  
responses: $y_i \in \mathbb{R}, i = 1, \ldots, N$

$l &ge; 0: complexity parameter,  
0 &ge; ɑ &ge; 1: how much ridge regression
$/widec$
$/eq$

### Commonly used function arguments

`alpha`
- for the elastic net mixing parameter $\alpha$
	- range: $\alpha \in [0,1]$
- $\alpha = 1$: lasso regression (default)
- $\alpha = 0$: ridge regression

`weights`
- for the observation weights
- default = 1 for each observation
- /!\ `glmnet` rescales the weights internally to sum to $N$, the sample size

`nlambda`
- the number of $\lambda$ values in the sequence
- default = 100

`lambda`
- can be provided if the user wants to specify the lambda sequence
	- but: typical usage is for the program to construct the lambda sequence on its own

`lambda.max` / `lambda.min.ratio`
- ratio of smallest value of the generated $\lambda$ sequence ("`lambda.min`") to `lambda.max`

$info$
`lambda.max` is not user-specified but is computed from the input $x$ and $y$: it is the smallest value for lambda such that all the coefficients are zero.
$/info$

`standardize`
- a logical flag for `x` variable standardization prior to fitting the model sequence
- coefficients are always returned on the original scale
- default = `TRUE`

$code$
**Example Gaussian glmnet call**

```r
wts <-  c(rep(1,50), rep(2,50))
fit <- glmnet(x, y, alpha = 0.2, weights = wts, nlambda = 20)
```

- $\alpha = 0.2$
- double weight to half the observations
- `nlambda` = 20 -> model fit is only computed for 20 values of $\lambda$

$info$
 In practice, we recommend `nlambda` to be 100 (default) or more. In most cases, it does not come with extra cost because of the warm-starts used in the algorithm, and for nonlinear models leads to better convergence properties.
$/info$

$widec$
$down
$/widec$

```r
print(fit)
```

$widec$
$down
$/widec$

```r
## 
## Call:  glmnet(x = x, y = y, weights = wts, alpha = 0.2, nlambda = 20) 
## 
##    Df  %Dev Lambda
## 1   0  0.00 7.9390
## 2   4 17.89 4.8890
## 3   7 44.45 3.0110
## 4   7 65.67 1.8540
## 5   8 78.50 1.1420
## 6   9 85.39 0.7033
## 7  10 88.67 0.4331
## 8  11 90.25 0.2667
## 9  14 91.01 0.1643
## 10 17 91.38 0.1012
## 11 17 91.54 0.0623
## 12 17 91.60 0.0384
## 13 19 91.63 0.0236
## 14 20 91.64 0.0146
## 15 20 91.64 0.0090
## 16 20 91.65 0.0055
## 17 20 91.65 0.0034
```

$info$
Here the actual number of $\lambda$
’s is less than that specified in the call. This is because of the algorithm’s stopping criteria. According to the default internal settings, the computations stop if either the fractional change in deviance down the path is less than ${10}^{-5}$
 or the fraction of explained deviance reaches ${0.999}$. From the last few lines of the output, we see the fraction of deviance does not change much and therefore the computation ends before the all 20 models are fit. 
$/info$
$/code$

### Predicting and plotting with `glmnet` objects

#### Common arguments

`s`
- for specifying the value(s) of $\lambda$ at which to extract coefficients/predictions

`exact`
- for indicating whether the _exact_ values of coefficients are desired or not

$info$
If `exact = TRUE` and predictions are to be made at values of `s` not included in the original fit, these values of `s` are merged with `object$‍lambda` and the model is refit before predictions are made.
$/info$

$code$
**Example**

```r
fit <- glmnet(x, y)
any(fit$‍lambda == 0.5)  # 0.5 not in original lambda sequence
```

$widec$
$down
$/widec$

```r
## [1] FALSE
```

----

```r
coef.apprx <- coef(fit, s = 0.5, exact = FALSE)
coef.exact <- coef(fit, s = 0.5, exact = TRUE, x=x, y=y)
cbind2(coef.exact[which(coef.exact != 0)], 
       coef.apprx[which(coef.apprx != 0)])
```

$widec$
$down
$/widec$

```r
##            [,1]       [,2]
## [1,]  0.2613110  0.2613110
## [2,]  1.0055470  1.0055473
## [3,]  0.2677140  0.2677134
## [4,] -0.4476485 -0.4476475
## [5,]  0.2379287  0.2379283
## [6,] -0.8230862 -0.8230865
## [7,] -0.5553678 -0.5553675
```

|left column|right column|
|---|---|
|`exact = TRUE`|`exact = FALSE`|

We see from the above that 0.5 is not in the sequence and that hence there are some small differences in coefficient values. Linear interpolation is usually accurate enough if there are no special requirements. Notice that with `exact = TRUE` we have to supply by named argument any data that was used in creating the original fit, in this case `x` and `y`.
$/code$

#### Predicting

`newx`
- matrix of new values for `x` at which predictions are desired

`type`
- allows users to choose the type of prediction returned
- `"link"`: returns the fitted values $\hat\beta_0 + x_i^T\hat\beta$
- `"response`: same output as `"link"` for `"gaussian"` family
- `"coefficients`: returns the model coefficients
- `"nonzero"`: returns a list of the indices of the nonzero coefficients for each value of `s`

$code$
**Example**

Get fitted values for the first 5 observations at $\lambda = 0.05$

```r
predict(fit, newx = x[1:5,], type = "response", s = 0.05)
```

$widec$
$down
$/widec$

```r
##              s1
## [1,] -1.3362652
## [2,]  2.5894245
## [3,]  0.5872868
## [4,]  2.0977222
## [5,]  1.6436280
```

$info$
If multiple values of `s` are supplied, a matrix of predictions is produced. If no value of `s` is supplied, a matrix of predictions is supplied, with columns corresponding to all the lambdas used in the fit.
$/info$
$/code$

#### Plotting

`xvar`
- decide what is plotted on the x-axis
- `"norm"`: the $\ell_1$-norm of the coefficients (default)
- `"lambda`: log-lambda value
- `"dev"`: %deviance explained

`label`
- whether to label the curves with the variable index number

$code$
```r
plot(fit, xvar = "lambda", label = TRUE)
```

$gallery$
![Image](img$yb2y)
$/gallery$

```r
plot(fit, xvar = "dev", label = TRUE)
```

$gallery$
![Image](img$s20h)

- %deviance: a very different picture.
- this is ==percent deviance explained== on the training data -> a measure of **complexity of the model**.
- toward the end of the path, %deviance is not changing much, but the coefficients are "blowing up" a bit
- => enables us focus attention on the parts of the fit that ==matter==
	- will especially be true for other models, such as logistic regression.
$/gallery$
$/code$

### Cross-validation

#### Arguments

`nfolds`
- the number of folds for the cross-validation

`foldid`
- user-supplied folds

`type.measure`
- the loss used for cross-validation
- `"deviance"` / `"mse"`: squared loss
- `"mae"`: mean absolute error

$code$
**Example**

```r
cvfit <- cv.glmnet(x, y, type.measure = "mse", nfolds = 20)
```

- 20-fold cross-validation
- based on mean-squared error criterion (default for `"gaussian"`)

$widec$
$down
$/widec$

```r
print(cvfit)
```

$widec$
$down
$/widec$

```r
## 
## Call:  cv.glmnet(x = x, y = y, type.measure = "mse", nfolds = 20) 
## 
## Measure: Mean-Squared Error 
## 
##      Lambda Index Measure     SE Nonzero
## min 0.07569    34   1.063 0.1135       9
## 1se 0.14517    27   1.164 0.1297       8
```

$info$
**Interesting!**

Shows you the *index* of the lambda values, which you can use later for different purposes.
$/info$
$/code$

#### Parallel computing

$warn$
Unfortunately, the package `doMC` is not available on Windows platforms (it is on others), so we cannot run the code here, but we present timing information recorded during one of our test runs.
$/warn$

$code$
```r
library(doMC)
registerDoMC(cores = 2)
X <- matrix(rnorm(1e4 * 200), 1e4, 200)
Y <- rnorm(1e4)
```

----

```r
system.time(cv.glmnet(X, Y))
```

$widec$
$down
$/widec$

```r
##    user  system elapsed 
##   2.440   0.080   2.518
```

```r
system.time(cv.glmnet(X, Y, parallel = TRUE))
```

$widec$
$down
$/widec$

```r
##    user  system elapsed 
##   2.450   0.157   1.567
```

- register a parallel worker beforehand (here: 2 cores)
- enable `parallel=TRUE`

$info$
Parallel computing can significantly speed up the computation process especially for large-scale problems.
$/info$
$/code$

#### `coef` / `predict`

parameter `s`
- here as well, `"lambda.min"` and `"lambda.1se"`

#### `foldid`

`foldid`
- allows you to control the fold that each observation is assigned to
- very useful when using cross-validation to select a value for $\alpha$!

$code$
```r
foldid <- sample(1:10, size = length(y), replace = TRUE)
cv1  <- cv.glmnet(x, y, foldid = foldid, alpha = 1)
cv.5 <- cv.glmnet(x, y, foldid = foldid, alpha = 0.5)
cv0  <- cv.glmnet(x, y, foldid = foldid, alpha = 0)
```

- $result thanks to `foldid`, you can be sure that in each call, the same folds are used!
$/code$

$code$
**Plotting all alpha trajectories**

```r
par(mfrow = c(2,2))
plot(cv1); plot(cv.5); plot(cv0)
plot(log(cv1$lambda)   , cv1$cvm , pch = 19, col = "red",
     xlab = "log(Lambda)", ylab = cv1$name)
points(log(cv.5$lambda), cv.5$cvm, pch = 19, col = "grey")
points(log(cv0$lambda) , cv0$cvm , pch = 19, col = "blue")
legend("topleft", legend = c("alpha= 1", "alpha= .5", "alpha 0"),
       pch = 19, col = c("red","grey","blue"))
```

$widec$
(there is no built-in code)
$/widec$

$gallery$
![Image](img$83wd)

1. lasso (`alpha=1`) does about the best here
2. the range of lambdas used differs with `alpha`
$/gallery$
$/code$

### Other function arguments

$acco$
`upper.limits`
- how large a coefficient can be

`lower.limits`
- how small a coefficient can be

$code$
```r
tfit <- glmnet(x, y, lower.limits = -0.7, upper.limits = 0.5)
plot(tfit)
```

- coefficients...
	1. ...must be bigger than -0.7
	2. ...must be smaller than 0.5

$gallery$
![image](img$tqny)
$/gallery$

$info$
these bounds **can be a vector**, with **different values for each coefficient**. if given as a scalar, the same number gets recycled for all.
$/info$
$/code$
$/acco$

$acco$
`penalty.factor`
- apply ==separate penalty factors to each coefficient==
- very useful when we have ==prior knowledge== or ==preference== over the variables
- default = 1 for each coefficient (coefficients are penalized equally)

$eq$
$$
\lambda \sum_{j=1}^p \boldsymbol{v_j} P_\alpha(\beta_j) = \lambda \sum_{j=1}^p \boldsymbol{v_j} \left[ (1-\alpha)\frac{1}{2} \beta_j^2 + \alpha |\beta_j| \right]
$$

$widec$
$v_j$: penalty factor for the $j$^th^ variable
$/widec$
$/eq$

$info$
This is useful in the case where some variables are always to be included unpenalized in the model, such as the demographic variables sex and age in medical studies.
$/info$

$warn$
The penalty factors are internally rescaled to sum to `nvars`, the number of variables in the given `x` matrix.
$/warn$

$code$
**Example**

- penalty factors for variables 1, 3 and 5 set to zero

```r
p.fac <- rep(1, 20)
p.fac[c(1, 3, 5)] <- 0
pfit <- glmnet(x, y, penalty.factor = p.fac)
plot(pfit, label = TRUE)
```

$gallery$
![Image](img$pvyl)
$/gallery$

- the three variables with zero penalty factors always stay in the model
- the others follow typical regularization paths and shrunk to zero eventually
$/code$
$/acco$

$acco$
`exclude`
- block certain variables from being in the model at all.

$info$Of course, one could simply subset these out of `x`, but sometimes exclude is more useful, since it returns a full vector of coefficients, just with the excluded ones set to zero.
$/info$
$/acco$

$acco$
`intercept`
- decide if an intercept should be included in the model or not (it is never penalized)
- default: `TRUE`
- `FALSE` = intercept forced to zero
$/acco$

## Linear Regression: `family = "mgaussian"` (multi-response)

### Introduction

multi-response Gaussian family
- useful when there are a number of (correlated) responses
- => "multi-task learning" problem

variable selection
- variable either *included* in the model for all responses, or *excluded* for all responses

$info$
Most of the arguments for this family are the same as that for `family = "gaussian"`, so we focus on the differences with the single response model.
$/info$

### Formal definition

response $y$
- ~~vector~~ -> matrix
- => coefficients at each value of lambda are *also* a matrix

$eq$
**Objective function**

$$
\begin{aligned}
\min_{(\beta_0, \beta) \in \mathbb{R}^{(p+1)\times K}}\frac{1}{2N} \sum_{i=1}^N  & \|y_i -\beta_0-\beta^T x_i\|^2_F+ \\ & \lambda \left[ (1-\alpha)\|\beta\|_F^2/2 + \alpha\sum_{j=1}^p\|\beta_j\|_2\right]
\end{aligned}
$$

$widec$
$\beta_j$: the $j$^th^ row of the $p \times K$ coefficient matrix $\beta$
$/widec$

$info$
We replace the absolute penalty on each single coefficient by a ==group-lasso penalty== on each coefficient $K$-vector $\beta_j$ for a single predictor (i.e. column of the `x` matrix). The group lasso penalty behaves like the lasso, but on the whole group of coefficients for each response: they are either all zero, or else none are zero, but are shrunk by an amount depending on $\lambda$.
$/info$
$/eq$

### Fitting and plotting

#### Fitting

$code$
**Example fit**

```r
data(MultiGaussianExample)
x <- MultiGaussianExample$x
y <- MultiGaussianExample$y
mfit <- glmnet(x, y, family = "mgaussian")
```

- we fit a regularized multi-response Gaussian model to the data
	- returned: object `mfit`
$/code$

`standardize.response`
- only for `mgaussian` family
- if `TRUE`, the response variables are standardized
- default = `FALSE`

$info$
The sample data contains **four** response variables.
$/info$

#### Plotting

$code$
**Example plot**

```r
plot(mfit, xvar = "lambda", label = TRUE, type.coef = "2norm")
```
$/code$

`type.coef = "2norm"`
- under this setting, a single curve is plotted per variable
- value = equal to $\ell_2$ norm of the variable's coefficient vector
- default = `"coef"` -> coefficient plot is created for each response (multiple figures)

### Extracting coefficients

$code$
**Extracting coefficients at different $\lambda$**

```r
coef(mfit, s = c(0.1, 0.01))
```

$widec$
$down
$/widec$

```r
## $y1
## 21 x 2 sparse Matrix of class "dgCMatrix"
##                       1            2
## (Intercept) -0.18950757 -0.186592903
## V1          -0.53764738 -0.530810613
## V2           0.80330316  0.916504435
## V3          -0.03789939 -0.084320266
## V4           0.02029550  0.020292160
## V5           1.14747018  1.244918525
## 
## $y2
## 21 x 2 sparse Matrix of class "dgCMatrix"
##                        1             2
## (Intercept) -0.137158403 -1.483021e-01
## V1           2.348036234  2.448750e+00
## V2           0.042364786  1.013759e-01
## V3           0.194999768  2.711751e-01
## V4          -0.573751783 -6.125152e-01
## V5          -0.013546491 -3.742899e-05
```
$/code$

### Prediction

$code$
**Extracting coefficients at different $\lambda$**

```r
predict(mfit, newx = x[1:5,], s = c(0.1, 0.01))

```

$widec$
$down
$/widec$

```r
## , , 1
## 
##              y1         y2         y3       y4
## [1,] -4.7106263 -1.1634574  0.6027634 3.740989
## [2,]  4.1301735 -3.0507968 -1.2122630 4.970141
## [3,]  3.1595229 -0.5759621  0.2607981 2.053976
## [4,]  0.6459242  2.1205605 -0.2252050 3.146286
## [5,] -1.1791890  0.1056262 -7.3352965 3.248370
## 
## , , 2
## 
##              y1         y2         y3       y4
## [1,] -4.6415158 -1.2290282  0.6118289 3.779521
## [2,]  4.4712843 -3.2529658 -1.2572583 5.266039
## [3,]  3.4735228 -0.6929231  0.4684037 2.055574
## [4,]  0.7353311  2.2965083 -0.2190297 2.989371
## [5,] -1.2759930  0.2892536 -7.8259206 3.205211
```

- result returned in 3D array
- first two dimensions: prediction matrix for each response variable
- third dimension: response variables (? lambda values)
$/code$

## Binomial Regression: `family = "binomial"`

### Introduction

logistic regression
- a widely-used model when the response is **binary**

$example$
**Example**

- response variables takes values $\mathcal{G}=\{1,2\}$
- denote $y_i = I(g_i=1)$

$widec$
$down
$/widec$

$$
\begin{aligned}
\operatorname{Pr}(G=2|X=x) &= \frac{e^{\beta_0+\beta^Tx}}{1+e^{\beta_0+\beta^Tx}} \\
\log\frac{\operatorname{Pr}(G=2|X=x)}{\operatorname{Pr}(G=1|X=x)} &= \beta_0+\beta^Tx,
\end{aligned}
$$

- $result second operation = log-odds transformation
$/example$

### Formal definition

$eq$
**Objective function**

$$
\begin{aligned}
\min_{(\beta_0, \beta) \in \mathbb{R}^{p+1}} & \underbrace{ - \left[ \frac{1}{N} \cdot \sum_{i=1}^N y_i \cdot (\beta_0 + x_i^T \cdot \beta) - \log (1+e^{(\beta_0+x_i^T \cdot \beta)})\right] }_{\text{negative log-likelihood of logistic regression}} + \\
& \underbrace{\lambda \big[ (1-\alpha)\|\beta\|_2^2/2 + \alpha\|\beta\|_1\big]}_{\text{regularization penalty}}
\end{aligned}
$$

$columns$
$wide$
**Negative log-likelihood**

- 𝑁: number of observations
- $\frac{1}{N}$: normalise loss (generalises across datasets),
- $\sum_{i=1}^N$: for each observation
- 𝑦~𝑖~: binary response variable for the 𝑖^th^ observation
- 𝑥~𝑖~: vector of predictor variables for the 𝑖^th^ observation
- $\beta_0$: intercept
- $\beta$: vector of regression coefficients
$/widec$
$wide$
**Regularization penalty**

- $l: controls the strength of the penalty
- ɑ: determines the balance between L1 and L2 regularisation
- L1: encourages sparsity in the model  
	- adds the sum of the absolute values of the coefficients as a penalty term
	- = $\|x\|_1$ with $x \in \mathbb{R}^n$ = $\|x_1\| + ... + \|x_n\|$
- L2: adds the sum of the squared coefficients
	- = $\|x\|_2$ with $x \in \mathbb{R}^n$ = $\sqrt{x_1^2 + ... + x_n^2}$
	- this is the same as the Euclidean length!
	- division by 2 = balances the regularization penalty
$/wide$
$/columns$

$info$
A ==vector norm== is *any* function we can use to ==measure the magnitude (or length) of a vector==. This concept extends the notion of an absolute value to vectors. [(Advanced Linear Algebra, UTexas)](https://www.cs.utexas.edu/users/flame/laff/alaff/chapter01-what-is-a-vector-norm.html)
$/info$

$info$
The logistic regression model estimates the probability of observing $y_i=1$ as a function of the predictors $x_i$, and this probability is modelled using the logistic function ($see $down):

$$
p(y_i=1|x_i) = \frac{1}{1+e^{-(\beta_0+x_i^T \beta)}}
$$
$/info$

$info$
The objective function is minimized with respect to the coefficients $\beta_0$ and $\beta$ to obtain the optimal values that minimize the negative log-likelihood of the logistic regression model subject to the regularization penalty.
$/info$
$/eq$

problems with logistic regression
- degeneracies when $p > N$ (number of variables / number of attestations)
- exhibits wild behaviour even when $N$ is close to $p$

elastic net penalty
- alleviates these issues
- regularises and selects variables
- => thanks, very cool

### Fitting

$code$
**Example dataset**

```r
data(BinomialExample)
x <- BinomialExample$x
y <- BinomialExample$y
```

$widec$
(100 observations)
$/widec$

- `x`: 30 variables for all observations
- `y`: response variable (0/1) for each observation
$/code$

$code$
**Fitting**

```r
fit <- glmnet(x, y, family = "binomial")
```

- `family="binomial"`: use logistic regression

$widec$
$down
$/widec$

```r
print(fit)
```

$widec$
$down
$/widec$

```r
##    Df  %Dev   Lambda
## 1   0  0.00 0.240500
## 2   1  2.90 0.219100
## 3   1  5.34 0.199600
## 4   2  8.86 0.181900
## 5   2 11.95 0.165800
```

- same properties as before
$/code$

### Plotting

$code$
```r
plot(fit)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$d38l)
$/gallery$
$/code$

### Prediction

$code$
```r
predict(fit, newx = x[1:5,], type = "class", s = c(0.05, 0.01))
```

$widec$
$down
$/widec$

```r
##      s1  s2 
## [1,] "0" "0"
## [2,] "1" "1"
## [3,] "1" "1"
## [4,] "0" "0"
## [5,] "1" "1"
```
$/code$

`type`
- allows users to choose the type of prediction returned
- `"link"`: returns the linear predictors
- `"response`: gives the fitted probabilities
- `"class`: produces the class label corresponding to the maximum probability
- `"coefficients"`: computes the coefficients at values of `s`
- `"nonzero"`: returns a list of the indices of the nonzero coefficients for each value of `s`

$info$
Note that the ==results== (“link”, “response”, “coefficients”, “nonzero”) are ==returned only for the class corresponding to the _second_ level of the factor response==.
$/info$

### Cross validation

#### Arguments

`type.measure`
- `"mse"`: squared loss
- `"deviance"`: actual deviance
- `"mea"`: mean absolute error
- `"class`: misclassification error
- `"auc"`: gives area under the ROC curve

$code$
```r
cvfit <- cv.glmnet(x, y, family = "binomial", type.measure = "class")
```

- misclassification error used as the criterion for 10-fold cross-validation
$/code$

$info$
==Misclassification error== reports the **fraction of misclassified predictions**.

The logic is: if the predicted probability is higher than 0.5, we assume class membership of the predicted class.

```r
## Measure: Misclassification Error 
## 
##     Lambda Index Measure      SE Nonzero
## min 0.1065    84    0.12 0.04341      30
## 1se 0.6240    65    0.16 0.03697      30
```

- $result so here, 16% of cases were misclassified for the 1se-model
$/info$

#### Plotting

$code$
**Plotting the optimal values of $\lambda$**

```r
plot(cvfit)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$k2ig)
$/gallery$

|`cvfit$‍lambda.min`|`cvfit$‍lambda.1se`|
|---|---|
|`0.02140756`|`0.04945423`|
$/code$

#### Offsets

"offset"
- a fixed vector of $N$ numbers that is added into the linear predictor

$example$
For example, you may have fitted some other logistic regression using other variables (and data), and now you want to see if the present variables can add further predictive power. To do this, you can use the predicted logit from the other model as an offset in the glmnet call.
$/example$

## Multinomial Regression: `family = "multinomial"`

### Introduction

multinomial model
- extends the binomial when the number of classes is more than two

$example$
- suppose the response variable has $K$ levels ${\mathcal G}=\{1,2,\ldots,K\}$
- to model:

$$
\operatorname{Pr}(G=k|X=x)=\frac{e^{\beta_{0k}+\beta_k^Tx}}{\sum_{\ell=1}^Ke^{\beta_{0\ell}+\beta_\ell^Tx}}.
$$

- There is a linear predictor for each class!
$/example$

### Formal definition

$eq$
**Objective function**

$$
\begin{aligned}
& \ell(\{\beta_{0k},\beta_{k}\}_1^K) = \\
& -\left[\frac{1}{N} \sum_{i=1}^N \Big(\sum_{k=1}^Ky_{il} (\beta_{0k} + x_i^T \beta_k)- \log \big(\sum_{\ell=1}^K e^{\beta_{0\ell}+x_i^T \beta_\ell}\big)\Big)\right] + \\ 
& \lambda \left[ (1-\alpha)\|\beta\|_F^2/2 + \alpha\sum_{j=1}^p\|\beta_j\|_q\right]
\end{aligned}
$$

$widec$
don't even worry about the notation, this is beyond you at this point
$/widec$
$/eq$

### Fitting

$code$
**Example data**

```r
data(MultinomialExample)
x <- MultinomialExample$x
y <- MultinomialExample$y
```

$widec$
(500 observations)
$/widec$

$widec$
$down
$/widec$

- `x`: 30 variables for all observations
- `y`: response variable (1/2/3) for each observation
$/code$

response variable possibilities
- can be `nc >= 2` level factor
- or: `nc`-column matrix of counts or proportions

$info$
Internally, `glmnet` will make the rows of this matrix sum to 1, and absorb the total mass into the weight for that observation. 
$/info$

`type.multinomial`
- `"ungrouped"`: no grouped lasso penalty (default)
- `"grouped"`: grouped lasso penalty

$code$
**Fitting**

```r
fit <- glmnet(x, y, family = "multinomial", type.multinomial = "grouped")
plot(fit, xvar = "lambda", label = TRUE, type.coef = "2norm")
```

$widec$
$down
$/widec$

$gallery$
![Image](img$ib1m)
$/gallery$

$info$
The same grouping arguments apply as for multi-Gaussian.
$/info$
$/code$

### Cross-validation

$info$
Although `type.multinomial` is not a named argument in `cv.glmnet`, in fact any argument that can be passed to `glmnet` is valid in the argument list of `cv.glmnet`. Such arguments are passed via the `...` argument directly to the calls to glmnet inside the `cv.glmnet` function.
$/info$

$code$
**Cross validation**

```r
cvfit <- cv.glmnet(x, y, family = "multinomial", type.multinomial = "grouped")
plot(cvfit)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$9itg)
$/gallery$
$/code$

### Prediction

$code$
```r
predict(cvfit, newx = x[1:10,], s = "lambda.min", type = "class")
```

$widec$
$down
$/widec$

```r
##       1  
##  [1,] "3"
##  [2,] "2"
##  [3,] "2"
##  [4,] "3"
##  [5,] "1"
##  [6,] "3"
##  [7,] "3"
##  [8,] "1"
##  [9,] "1"
## [10,] "2"
```
$/code$