$public=true$

# 02. Lasso Regression

## Overview of regressions

|regular linear regression|ridge regression|lasso regression|
|---|---|---|
|![Image](img$bnhf)|![Image](img$ryuf)|![Image](img$pby8)|
|minimises:|minimises (the sum of):|minimises (the sum of):|\
|- the sum of the squared residuals|- the sum of the squared residuals|- the sum of the squared residuals|\
| |- $\lambda \cdot (slope^2 + slope^2 + ...)$|- $\lambda \cdot \\|slope + slope + ...\\|$|

no _squared_ slope
- instead: **absolute value**
- $\lambda + \\|slope + slope + ...\\|$

$result $\lambda$
- can be any value from $\\0$ to $+\infty$
- determined also using **cross-validation**, as with ridge regression ($see <-)

$info$
When ridge and lasso regression shrink parameters, they don't have to shrink them all equally.
$/info$

## Comparison to ridge regression

### Similarities

$columns$
$acco$
$wide$
**1.** making predictions less sensitive to tiny training data set

- penalty affects slopes
- penalty can be varied according to training data
- prevent overfitting
$/wide$
$/acco$

$acco$
$wide$
**2.** can be applied in the same contexts
- continuous
- discrete
- logistic
$/wide$
$/acco$
$/columns$

$gallery port$
$widec$
![Image](img$hy5c)
$/widec$
$/gallery port$

### Differences

|ridge regression|lasso regression|
|---|---|
|high $l values can bring the slope **asymptotically close** to zero|high $l values can bring the slope **completely** to zero|
|a little better (than $see ->) when most variables are useful|a little better (than $see <-) in models that contain a lot of useless variables|

$example$
$$
\begin{aligned}
\textbf{Size} = y-intercept + slope \cdot \textbf{Weight} + \text{diet difference} \cdot \textbf{High Fat Diet} + \\ \text{astrological offset} \cdot \textbf{Sign} + \text{airspeed scalar} \cdot \textbf{Airspeed of Swallow}
\end{aligned}
$$

- "astrological sign" and "airspeed of a swallow" are terrible ways to predict size

- ridge regression:  
	$\lambda + (slope^2 + \text{diet difference}^2 + \text{astrological offset}^2 + \text{airspeed scalar}^2)$
	- useless parameters might shrink a lot, but they will never be equal to 0
- lasso regression:  
	$\lambda + (|slope| + |\text{diet difference}| + |\text{astrological offset}| + |\text{airspeed scalar}|)$
	- useless parameters shrink down all the way to 0
	- => we're left with a way to predict **Size** that only includes **Weight** and **Diet**
$/example$