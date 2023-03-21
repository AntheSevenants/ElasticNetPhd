$public=true$

# 03. Elastic-Net Regression

## Deciding between ridge regression and lasso regression

### Overview of strengths

|ridge regression|lasso regression|
|---|---|
|high $l values can bring the slope **asymptotically close** to zero|high $l values can bring the slope **completely** to zero|
|a little better (than $see ->) when most variables are useful|a little better (than $see <-) in models that contain a lot of useless variables|

### Automatic decision making

#### Overview

models with *many* variables/parameters
- we don't *know* beforehand which parameters will be useful
- how to choose?

$result solution
- you **don't** -> **Elastic Net Regression**

$widec$
$down
$/widec$

$eq$
$$
\textcolor{#F98003}{\underbrace{\lambda_1 \cdot |variable_1| + ... + |variable_X|}_{\text{Lasso regression penalty}}} + \textcolor{#198FE3}{\underbrace{\lambda_2 \cdot variable_1^2 + ... + variable_X^2}_{\text{Ridge regression penalty}}}
$$
$/eq$

$warn$
The <span style="color: #F98003; background-color: #fff;">lasso regression penalty</span> and the <span style="color: #198FE3; background-color: #fff;">ridge regression penalty</span> get their own $ls.
$/warn$

#### Finding $lambda

finding $ls
- we *still* use cross-validation
- we find **combinations** of $l~1~ and $l~2~

$info$
The hybrid **Elastic-Net Regression** is especially good at dealing with situations where there are correlations between parameters:

- on its own, Lasso Regression tends to pick just one of the correlated terms, and eliminates the others
- on its own, Ridge Regression tends to shrink *all* of the parameteres for the correlated variables together

By combining Lasso and Ridge Regression, Elastic-Net Regression groups and shrinks the parameters associated with the correlated variables and leaves them in the equation, *or* it removes them all at once.
$/info$

#### Software implementation

$info$
In practice, our $\lambda_1$ and $\lambda_2$ from $see $up are related through a single parameter $\alpha$. Threre is only a single $\lambda$ parameter (which will be computed automatically through _k-fold_), and $\alpha$ will correspond to the amount of lasso regression that is used (ranging from 0 to 1):

$eq$
$$
\lambda \cdot \left[ \textcolor{#F98003}{\alpha \cdot \underbrace{\left( |variable_1| + ... + |variable_X| \right) }_{\text{Lasso regression penalty}}} + \textcolor{#198FE3}{(1 - \alpha) \cdot \underbrace{\left( variable_1^2 + ... + variable_X^2 \right)}_{\text{Ridge regression penalty}}} \right]
$$
$/eq$

To summarise, when we use an Elastic Net implementation (like `glmnet`), we will test different values for both $\lambda$ and $\alpha$.
$/info$