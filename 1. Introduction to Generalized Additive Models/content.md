$public=true$

# 1. Introduction to Generalized Additive Models

$wide$
from ["Introduction to Generalized Additive Models" by Noam Ross](https://noamross.github.io/gams-in-r-course/chapter1)
$/wide$

$p=1$

## Introduction

### Comparison to linear regression models

linear regression models
- easy to interpret and use for inference
- easy to understand meaning of the parameters
- but: not apt to model complex phenomena

machine learning models
- e.g. boosted regression trees, neutral network
- very good at making predictions of complex relationships
- problems:
	1. need lots of data
	2. quite difficult to interpret
	3. one can rarely make inferences from the model results

GAMs
- a middle ground
- can be fit to complex, non-linear relationships
- make good predictions
- still possible to do inferential statistis
	- one can still understand and explain the underlying structure of our models
	- we can understand why they predict something

$gallery port$
**Example of a non-linear relationship**

$widec$
![Image](img$h3xv)
$/widec$

$widec$
$down
$/widec$

$widec$
![Image](img$meag)
![Image](img$znbb)
$/widec$

$columns$
$widec$
distinction would get ==lost in a linear model==
$/widec$

$widec$
==GAMM== makes a ==much better prediction==
$/widec$
$/columns$
$/gallery port$

### Composition of GAMs

flexible smooth of a GAM
- actually constructed of ==many smaller functions== (= *basis functions*)
- = the sum of a number of basis functions
- each ==basis function== is ==multiplied by a coefficient==
	- = a parameter in the model

final smooth
- a single non-linear relationship between a dependent and independent variable has...
	1. several parameters
	2. an intercept.
- different, and more complex, than a linear model
	- here: each variable has only a single coefficient or parameter

$p=3$

### Example: motorcycle crash data

$code$
```r
mcycle <- MASS::mcycle
 
# Load mgcv
library(mgcv)
 
# Fit the model
gam_mod <- gam(accel ~ s(times), data = mcycle)
 
# Plot the results
plot(gam_mod, residuals = TRUE, pch = 1)
```

$widec$
$down
$/widec$

$gallery port$
$widec$
![Image](img$7424)
$/widec$
$/gallery port$
$/code$

$p=4$

## Parts of a non-linear function

`coef()` function
- extracts the coefficients of thebasis functions the GAM model object

$code$
```r
library(mgcv)

mcycle <- MASS::mcycle
gam_mod <- mgcv::gam(accel ~ s(times), data=mcycle)

# Extract the model coefficients
coef(gam_mod)
```

$widec$
$down
$/widec$

```
Loading required package: nlme
This is mgcv 1.8-26. For overview type 'help("mgcv-package")'.
(Intercept)
-25.5458646616541
s(times).1
-63.7180075250086
s(times).2
43.4756443634912
s(times).3
-110.350131585749
s(times).4
-22.181006356226
s(times).5
35.0344227720541
s(times).6
93.1764583974546
s(times).7
-9.28301845598693
s(times).8
-111.661471743285
s(times).9
17.6037820247035
```

$wide$
- smooth is made up of ==9 basis functions==, each with their ==own coefficient==
- GAM with just two variables can have many coefficients, which means they ==require a bit more data than linear models==
$/wide$
$/code$

$p=5$

## Basis functions and smoothing

end smooth
- close to the data (avoiding _under_-fitting)
- not fitting the noise (avoiding _over_-fitting)

$widec$
$down
$/widec$

$eq$
**Balancing wiggliness**

$bigtex$
$$
\text{Fit} = \text{Likelihood} - \lambda \cdot Wiggliness
$$
$/bigtex$

- *likelihood*: how well the GAM captures patterns in the data
- *complexity* / *wigliness*: how much the curve changes shape
- => good fit = good trade-off between the likelihood and complexity
	- $l: controls the balance
$/eq$

$gallery$
**Choosing the right smoothing parameter**

$widec$
plot with different smoothing values
$/widec$

![Image](img$diyp)

$columns$
$widec$
left = too much smoothing
$/widec$

$widec$
middle = too little smoothing
$/widec$

$widec$
right = good!
$/widec$
$/columns$
$/gallery$

### Setting a smoothing parameter

`sp`
- smoothing parameter in R
- can be set for a specific smooth, or for the whole model

$code$
$columns$
$acco$
**for whole model**

```r
gam(y ~ s(x), data = dat, sp = 0.1)
```$/acco$

$acco$
**for a specific term**

```r
gam(y ~ s(x, sp = 0.1), data = dat)
```
$/acco$
$/columns$
$/code$

$info$
**Best way of choosing smoothing parameter**

Just let REML / "Restricted Maximum Likelihood" method handle it for you:

```r
gam(y ~ s(x), data=dat, method="REML")
```
$/info$

### Choosing the number of basis functions

number of basis functions
- also dictate how wiggly a GAM function can be

$gallery$
![Image](img$luwh)

- smooth with a small number of basis functions: limited wiggliness
- smooth with with many basis functions: capable of capturing finer patterns

$widec$
$down
$/widec$

- value too low: model will not be wiggly enough
- value too high: noise is modelled, harder to converge ... problems
$/gallery$

$code$
```r
gam(y ~ s(x, k=3), data=dat, method="REML")
gam(y ~ s(x, k=10), data=dat, method="REML")
```

- you can let the algorithm decide the 𝑘 by itself (just don't enter 𝑘)

$gallery port$
$widec$
![Image](img$mgip)
$/widec$

$columns$
$widec$
left = too few functions
$/widec$

$widec$
right = adequate functions
$/widec$
$/columns$
$/gallery port$
$/code$

$p=9$

## Multivariate GAMs

multivariate GAMs
- GAMs with more than one predictor
- can contain a mixture of smooths, linear effects, continuous / categorical variables

### Univariate GAM

$example$
**Example: highway fuel efficiency by car weight**

$code$
```r
model <- gam(hw.mpg ~ s(weight), data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$ey4i)

- ==non-linear, decreasing relationship== between the two variables
$/gallery$
$/example$

### Step towards multivariate GAMs

adding an extra term
- easy! just add another `s()`

$example$
**Example: highway fuel efficiency by car weight *and* length**

$code$
```r
model <- gam(hw.mpg ~ s(weight) + s(length), data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$axg0)

- ==length has increasing non-linear effect== on fuel economy
	- effect is ==weaker== than the weight effect
$/gallery$

$info$
In this model, both the effect of weight and price are non-linear terms, but the two are ==simply added together to get a final prediction==. That addition is where the ==_additive_== in generalized _additive_ models comes from.
$/info$
$/example$

### Adding linear terms

linear terms
- same as adding other smooths, but without the `s()`
- especially useful for ==categorical variables== -> they are inherently ==linear== ($see $down)

$example$
**Example: highway fuel efficiency by car weight *and* length (linear)**

$code$
```r
model2 <- gam(hw.mpg ~ s(weight) + length, data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$0x5p)
$/gallery$

$warn$
In practice, ==we rarely make continuous variables linear in GAMs==. This is because, ==if the relationship is really linear==, or there is not enough data to show otherwise, ==automatic smoothing will force a linear shape==.
$/warn$
$/example$

### Adding categorical terms

categorical terms
- just regular, linear terms

$example$
**Example: highway fuel efficiency by car weight *and* fuel type (categorical)**

$code$
```r
model3 <- gam(hw.mpg ~ s(weight) + fuel, data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$fs0l)
$/gallery$

- two fuel categories: `diesel` and `gas`
- linear term with categorical variable results in **fixed effect** for each level of the category
- here: gasoline = negative effect, diesel = positive effect (more fuel efficient)

$info$
In this model, the non-linear effect of weight applies to vehicles of *both* gas types. This is the non-linear equivalent to a ==fixed slope, varying intercepts model== in linear regression.
$/info$

$warn$
When you use categorical variables this way, it's important that the variables are stored as ==factors==. The `mgcv` package does not use character variables.
$/warn$
$/example$

### Adding categorical terms with factor-smooth interaction

factor-smooth interaction
- will fit ==different smooths for different categorical variables==
- uses the `by` argument to the `s()` function -> calculate a different smooth for each unique category

$example$
**Example: highway fuel efficiency by car weight *and* fuel type (factor-smooth interaction)**


$code$
```r
model4 <- gam(hw.mpg ~ s(weight, by=fuel), data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$fe9y)
$/gallery$

- we get different smooths for diesel and gas cars
- /!\ the diesel smooth is much more uncertain
$/example$

$info$
**Varying intercept**

Usually, when we have smooth-factor interactions, we want to also include a ==varying intercept==, ==in case the different categories are **different in overall means** in addition to shape of their smooths==. Here, you see adding this varying intercept improves the estimate of the smooth for diesel cars:

$code$
```r
model4b <- gam(hw.mpg, s(weight, by=fuel) + fuel,
			   data=mpg, method="REML")
```
$/code$

$gallery$
![Image](img$ccls)
$/gallery$
$/info$

$p=12$

$example$
**Elaborate example**

- model predicts city fuel efficiency (`city.mpg`)
- smooth terms of: `weight`, `length` and `price`
- but: each of the smooth terms depends on the `drive` categorical variable

$code$
```r
library(gamair)
data("mpg", package="gamair")
  
library(mgcv)
  
# Fit the model
mod_city3 <- gam(city.mpg ~ 
				 s(weight, by = drive) + 
				 s(length, by = drive) + 
				 s(price, by = drive) + drive,
                data = mpg, method = "REML")
  
# Plot the model
plot(mod_city3, pages = 1)
```
$/code$

$gallery$
![Image](img$fr0i)

- each smooth is broken up into three different smooths for each `drive` category
- (I think the drive categorical intercepts are hidden)
$/gallery$
$/example$