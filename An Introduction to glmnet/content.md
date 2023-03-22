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