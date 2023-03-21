$public=true$

# 01. Ridge Regression

## Dataset

$gallery port$
**Dataset: weight and size of mice**

$widec$
![Image](img$1jhw)
$/widec$
$/gallery port$

## Simple regression

### Linear regression

$gallery port$
$widec$
![Image](img$slt3)
$/widec$

- we use **linear regression** to fit a **least-squares line**
- we find the following equation: 

$$
\textbf{Size} = \textcolor{orange}{0.9} + \textcolor{blue}{0.75} \cdot \textbf{Weight}
$$

$widec$
$result <span style="color: orange;">intercept</span> and <span style="color: blue;">slope</span> are deduced from the data
$/widec$
$/gallery port$

$info$
When we have a lot of measurements, we can be fairly confident that the **Least Squares** line accurately reflects the relationship between **Size** and **Weight**.
$/info$

### Linear regression: few data points

$gallery port$
**Linear regression with few data points?**

$widec$
![Image](img$t03c)
$/widec$

$widec$
$down
$/widec$

$widec$
![Image](img$ih7j)
$/widec$

$widec$
$down new equation
$/widec$
$$
\textbf{Size} = \textcolor{orange}{0.4} + \textcolor{blue}{1.3} \cdot \textbf{Weight}
$$

$widec$
$result <span style="color: green;">intercept</span> and <span style="color: blue;">slope</span> are again deduced from the data, but they are very skewed!
$/widec$
$/gallery port$

### Comparing full and partial data sets

$gallery$
|<span style="color: #FF0000; background-color: #fff;">training data</span>|<span style="color: #80E789;">testing data</span>|
|---|---|
|![Image](img$08qd)|![Image](img$92aj)|
|sum of squared residuals for <span style="color: #FF0000;">training data</span> = small (~ 0)|sum of squared residuals for <span style="color: #80E789; background-color: #111;">testing data</span> = large|
|/!\ from the perspective of the <span style="color: red;">bad slope</span>||

$widec$
$see $down

<span style="color: #FF0000;">new line</span> has **high variance** and is **overfit** to the <span style="color: #FF0000;">training data</span>
$/widec$
$/gallery$

$deflist$
**Reminder: variance**

variance is "the expectation of the squared deviation of a random variable from its population mean or sample mean" [(Wikipedia)](https://en.wikipedia.org/wiki/Variance) -- in this case, we interpret variance from the perspective of the <span style="color: red;">bad regression slope</span>
$/deflist$

## Ridge regression

### The idea behind ridge regression

ridge regression
- find a <span style="color: #008FE0;">new line</span> that _doesn't_ fit the <span style="color: #FF0000;">training data</span> as well
- we introduce a small amount of **bias** into how the <span style="color: #008FE0;">new line</span> is fit to the data
- in return: significant drop in variance
- => by starting with a slightly worse fit, ridge regression can provide **better long-term predictions**

$gallery port$
$widec$
![Image](img$wcc0)
$/widec$

$widec$
<span style="color: #008FE0;">new line</span>
$/widec$
$/gallery port$

### How ridge regression works

$$
\textbf{Size} = \text{y-axis intercept} + slope \cdot \textbf{Weight}
$$

$widec$
$down

regression formula remains the same, but *how* we compute the coefficients is different!
$/widec$

|regular linear regression|ridge regression|
|---|---|
|![Image](img$bnhf)|![Image](img$ryuf)|
|minimises:|minimises (the sum of):|\
|- the sum of the squared residuals|- the sum of the squared residuals|\
| |- $\lambda \cdot (slope^2 + slope^2 + ...)$|

$result $(slope^2 + slope^2 + ...)$
- adds a penalty to the traditional least squares method

$result $\lambda$
- determines how severe the penalty is
- can be any value from 0 to $+\infty$
	- /!\ 0 = penalty disabled!

$info$
**Multiple slopes**

You can also have multiple slopes, like in other regression models. We add the multiple slopes (squared) to each other ($see $up) and then apply the $\lambda$ penalty.

$gallery port$
$widec$
![Image](img$9kgn)
$/widec$
$/gallery port$
$/info$

#### The effect of ridge regression on the data

few data
- can distort the estimations of a least-squares regression model
- you can get **crazy slopes**, which means that small changes to the predictors can affect the response value a lot
- e.g. $see $down

$result gentle slopes
- do *not* have this problem!
- they are more conservative when it comes to their effect on response variables

$gallery port$
**Example of a tall and a gentle slope**

$widec$
![Image](img$w4wq)
![Image](img$2vfs)
$/widec$
$/gallery port$

use of ridge regression
- leads to a **gentler slope**

$gallery port$
**Example of a tall and a gentle slope**

$widec$
![Image](img$oyt0)
$/widec$
$/gallery port$

$info$
The larger we make $\lambda$, the closer the slope gets asymptotically to 0.

$gallery port$
$widec$
![Image](img$9csv)
$/widec$
$/gallery port$
$/info$

#### Finding $l

legal values for $l
- from $\\0$ to $+\infty$

$widec$
$down which value?
$/widec$

cross validation
- e.g. 10-fold cross validation
- we try a bunch of values for $\lambda$
- we check which value results in the lowest Variance (in the <span style="color: #80E789; background-color: #111;">testing data</span>)

$gallery port$
$widec$
![Image](img$4bb7)
$/widec$
$/gallery port$

### Discrete data

discrete data and ridge regression
- also works!
- you shimmy an intercept and slope

$gallery port$
$widec$
![Image](img$lftz)
$/widec$

- $result <span style="color: #80E789;">group 0</span> mean becomes the intercept
- $result difference between <span style="color: #80E789;">group 0</span> and <span style="color: #FF0000;">group 1</span> becomes the slope (~= t-test)
$/gallery port$

$gallery port$
**Computing the residuals**

$widec$
![Image](img$qnpx)
$/widec$

- $result Least squares for <span style="color: #80E789;">group 0</span>: differences between intercept and data
- $result Least squares for <span style="color: #FF0000;">group 1</span>: differences between intercept + pseudoslope and data
$/gallery port$

applying ridge regression
- works in the same way as for continuous data
- sum of square residuals + penalty of $see $up

$widec$
$down effect
$/widec$

$gallery port$
$widec$
![Image](img$9bbv)
$/widec$

- => pseudoslope shrinks
- => assumed difference between the two groups becomes smaller
$/gallery port$

$info$
The same formula still applies!
$/info$

### Logistic regression

----
$widec$
Ridge regression can also be applied to logistic regression.
$/widec$
----

$gallery$
![Image](img$tbdx)
$/gallery$

$info$
Logistic regression is solved using **maximum likelihood estimation**.
$/info$

## The use of ridge regression

### Theoretical example: only one data point

single data point
- problem -> how to compute the slope? we don't have enough information

$gallery port$
$widec$
![Image](img$ivcu)
$/widec$
$/gallery port$

### Theoretical example: three parameters, two data points

parameters required
- y-intercept
- slope for weight
- slope for age

$gallery port$
$widec$
![Image](img$7atc)
$/widec$

- $result we have two data points, but need to fit a plane to the data in three dimensions
- impossible to fit this plane, because not enough data points to infer the actual plane
$/gallery port$

$widec$
$down
$/widec$

curse of dimensionality
- you need at least as many data points as you have variables
- quickly becomes an issue when you have thousands of predictors
- ridge regression can fix this issue

### How ridge regression can help us

|single data point example|single data point + ridge regression + testing data|
|---|---|
![Image](img$v5op)|![Image](img$xto0)|
|/!\ problem! no clue what slope to fit|k-fold cross validation and regression penalty|\
| |- $result can estimate a good slope that still fits our test data|

$warn$
Only a theoretical example!
$/warn$