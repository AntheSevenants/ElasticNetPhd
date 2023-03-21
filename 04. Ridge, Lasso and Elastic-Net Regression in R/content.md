$public=true$

# 04. Ridge, Lasso and Elastic-Net Regression in R

## Elastic net regression

### Equation form

$eq$
$$
\textcolor{#F98003}{\underbrace{\lambda_1 \cdot (|variable_1| + ... + |variable_X|)}_{\text{Lasso regression penalty}}} + \textcolor{#198FE3}{\underbrace{\lambda_2 \cdot (variable_1^2 + ... + variable_X^2)}_{\text{Ridge regression penalty}}}
$$
$/eq$

### `glmnet` implementation

----
$widec$
There are no two $l!
$/widec$
----

$eq$
$$
\lambda \cdot \left[ \textcolor{#F98003}{\underbrace{\alpha \cdot (|variable_1| + ... + |variable_X|)}_{\text{Lasso regression penalty}}} + \textcolor{#198FE3}{\underbrace{(1 - \alpha) \cdot (variable_1^2 + ... + variable_X^2)}_{\text{Ridge regression penalty}}} \right]
$$
$/eq$

$l
- controls the penalty strength

ɑ
- can be any value from 0 to 1
- when ɑ = 0, only ridge regression is left
- when ɑ = 1, only lasso regression is left
- else, we get the mix of lasso and ridge

$gallery port$
$widec$
![Image](img$w5sd)
$/widec$

$widec$
We will test different values for **$l** and **ɑ**.
$/widec$
$/gallery port$

## Implementation in R

### Importing the library

$code$
```r
library(glmnet)  # Package to fit ridge/lasso/elastic net models
set.seed(42)  # Set seed for reproducibility
```
$/code$

### Generating a dataset

$code$
```r
n <- 1000  # Number of observations
p <- 5000  # Number of predictors included in model
real_p <- 15  # Number of true predictors

## Generate the data
# We generate n x p random values
x <- matrix(rnorm(n*p), nrow=n, ncol=p)
# The 'to model' variable will be the sum of the
# first 15 columns from the matrix.
# rnorm adds a bit of noise
y <- apply(x[,1:real_p], 1, sum) + rnorm(n)
```
$/code$

### Splitting the dataset

$code$
```r
## Split data into training and testing datasets.
## 2/3rds of the data will be used for Training and 1/3 of the
## data will be used for Testing.
train_rows <- sample(1:n, .66*n)
x.train <- x[train_rows, ]
x.test <- x[-train_rows, ]

y.train <- y[train_rows]
y.test <- y[-train_rows]
```
$/code$

### Cross-validation in action

#### Ridge regression

$code$
**Ridge regression (training)**

```r
alpha0.fit <- cv.glmnet(x.train, y.train, type.measure="mse", 
  alpha=0, family="gaussian")
```

- `cv`: cross-validation
- by default, 10-fold cross-validation is used
- we want to use `x.train` to predict `y.train`
- `type.measure`: how the cross-validation will be evaluated
	- `mse`: mean squared error (sum of the squared residuals divided by the sample size)
	- `deviance`: to be used with Elastic-Net regression for logistic regression
- `alpha`: 0 (to only have ridge regression)
- `family`
	- `gaussian`: do linear regression
	- `binomial`: do logistic regression

$warn$
Formula notation is not supported!
$/warn$
$/code$

$code$
**Ridge regression (testing)**

```r
alpha0.predicted <- 
  predict(alpha0.fit, s=alpha0.fit$lambda.1se, newx=x.test)
```

- we use `predict()` to try the **Testing** data
- first parameter: fitted model
- `s`: the *size* of the penalty -> set to one of the optimal values for $l stored in the model
	- alternatively, we could set `s` to **lambda.min**, which would be the $l that resulted in the smallest sum
- `newx`: testing dataset

```r
mean((y.test - alpha0.predicted)^2)
```

- we calculate the mean squared error of the difference between the true values (stored in `y.test`), and the predicted values
$/code$

$example$
**Mean squared error**

```

```
$/example$

#### Lasso regression

$code$
**Lasso regression (training)**

```r
alpha1.fit <- cv.glmnet(x.train, y.train, type.measure="mse", 
  alpha=1, family="gaussian")
```
$/code$

$code$
**Lasso regression (testing)**

```r
alpha1.predicted <- 
  predict(alpha1.fit, s=alpha1.fit$lambda.1se, newx=x.test)

mean((y.test - alpha1.predicted)^2)
```
$/code$

$example$
**Mean squared error**

```
1.19
```
$/example$

#### Elastic net regression (ɑ=0.5)

$code$
**Elastic net (training)**

```r
alpha0.5.fit <- cv.glmnet(x.train, y.train, type.measure="mse", 
  alpha=0.5, family="gaussian")
```
$/code$

$code$
**Elastic net (testing)**

```r
alpha0.5.predicted <- 
  predict(alpha0.5.fit, s=alpha0.5.fit$lambda.1se, newx=x.test)

mean((y.test - alpha0.5.predicted)^2)
```
$/code$

$example$
**Mean squared error**

```
1.246073
```
$/example$

#### Elastic net regression (ɑ=?)

$code$
```r
list.of.fits <- list()
for (i in 0:10) {
  ## Here's what's going on in this loop...
  ## We are testing alpha = i/10. This means we are testing
  ## alpha = 0/10 = 0 on the first iteration, alpha = 1/10 = 0.1 on
  ## the second iteration etc.
  
  ## First, make a variable name that we can use later to refer
  ## to the model optimized for a specific alpha.
  ## For example, when alpha = 0, we will be able to refer to 
  ## that model with the variable name "alpha0".
  fit.name <- paste0("alpha", i/10)
  
  ## Now fit a model (i.e. optimize lambda) and store it in a list that 
  ## uses the variable name we just created as the reference.
  list.of.fits[[fit.name]] <-
    cv.glmnet(x.train, y.train, type.measure="mse", alpha=i/10, 
      family="gaussian")
}
```

```r
## Now we see which alpha (0, 0.1, ... , 0.9, 1) does the best job
## predicting the values in the Testing dataset.
results <- data.frame()
for (i in 0:10) {
  fit.name <- paste0("alpha", i/10)
  
  ## Use each model to predict 'y' given the Testing dataset
  predicted <- 
    predict(list.of.fits[[fit.name]], 
      s=list.of.fits[[fit.name]]$lambda.1se, newx=x.test)
  
  ## Calculate the Mean Squared Error...
  mse <- mean((y.test - predicted)^2)
  
  ## Store the results
  temp <- data.frame(alpha=i/10, mse=mse, fit.name=fit.name)
  results <- rbind(results, temp)
}

## View the results
results
```
$/code$