$public=true$

# Investigating lexical effects in syntax with regularized regression (lasso)

$reader$
**Abstract**

Within usage-based theory, notably in construction grammar though also elsewhere,
the role of the lexicon and of lexically-specific patterns in morphosyntax is well recognized. The methodology, however, is not always sufficiently suited to get at the details,
as lexical effects are difficult to study under what are currently the standard methods
for investigating grammar empirically. In this short article, we propose a method
from machine learning: regularized regression (Lasso) with k-fold cross-validation,
and compare its performance with a Distinctive Collexeme Analysis.
$/reader$

## Introduction

### Influence of the lexicon on form

usage-based construction grammar
- no clear distinction between lexicon and syntax
- "It's constructions all the way down" (Goldberg 2006)
- => argument constructions and syntactic frames have their own meaning

$example$
**Example of constructions with overlapping meanings (dative alternance)**

- double object construction
- prepositional object construction

$wide$
- $result can both express transfer of possession, and can occur with verbs
like _give_, _donate_, _pass_, _transfer_, _send_ ...
- $result the choice is driven multifactorially (Bresnan et al., 2007 and the references mentioned above), by **animacy** and **topicality** among other factors
- /!\ at the same time: not all verbs that engage in this argument construction choice have the same weights for these factors => some verbs are more sensitive to these factors than others
$/wide$
- $result some verbs may have a very strong predilection for one of the variants
$/example$

verb meaning (in usage-based construction grammar)
- does not determine wholly the construction it combines with (<-> lexicalist/projectionist approaches)
- has to do with frequency of use, entrenchment, analogy
and partially also with ==lexical semantics==

### Investigating the influence

how to investigate?
- **integrate** the lexical semantic effects into the ==multifactorial== accounts
- e.g. linear regression (mostly with the logit link)

why linear regression?
- can deal with a multifactorial design combining both **extralinguistic**
predictors (such as age, gender, and socio-economic status) and **intralinguistic predictors**
- gives both **effect size** and **significance**
- can simultaneously integrate **numeric** and
**categorical** variables, and **interaction effects**
- => models achieve ==high levels of explained variance, as measured by R²==

### Issues with linear regression for lexical effects

----
$widec$
How to integrate lexical effects in linear regression analysis?
$/widec$
----

**1.** categorical predictor
- entering all the different verbs as levels of a categorical predictor in the analysis
- /!\ problem: ==most datasets are too small== to cope with the
inflation of the predictor set

**2.** building the statistical model for one verb only
- e.g. in the dative alternation, one could look solely at the prototypical example of _give_ in its transfer of possession sense
- 👍 advantage: the lexical effect is kept under tight control
- 👎 disadvantage: generalizability: the result is robust, but ==cannot be straightforwardly extrapolated to
other verbs==

**3.** building a mixed-effect model with ==different intercepts (/slopes) for different verbs==
- state-of-the-art solution in corpus linguistics today (Gries, 2015)
- 'random effects' -> necessary to be conservative about the effect of the focus predictors in the fixed effects
- 👍 advantage: great for avoiding Type I errors
- 👎 disadvantage: increase in Type II errors:
the amount of variance that is left to be explained by the fixed effects
may be too small to distinguish subtle effects
	 - this may be the case if
there is collinearity between the effects of different verbs and the focal
variables
	- the coefficients for the predictors are then partialled out over different lexemes that occur in the construction -> obscures the precise lexical
effects

### Using Lasso regression with cross-validation

$acco$
----
$widec$
Advantages
$/widec$
----

1. we can stick to the **regular linear regression design**
2. we retain **multivariate control** (with extra-linguistic and intra-linguistic factors) and interactions
3. we get a **generalisable method** that is more **robust against overfitting**
4. we are **not stretching the use of random effects** beyond what they are designed for
5. we are able to **increase the number of predictors** that can be entered into the regression
$/acco$

$acco$
----
$widec$
Disadvantages
$/widec$
----

1. we have to give up on the mixed-model design for *other* typical random factors
	- e.g. author, text, genre
	- solution $1: if the genre division is not too
fine-grained, it may be wiser to enter it as a fixed effect
	- solution $2: avoid using multiple observations from the
same text file
$/acco$

## Regularised Regression

### Overfitting

entering lemmas as fixed effects
- often not a sensible option in corpus studies for lexical effects
- if the lemma is treated as a factor with all lexemes as
factor levels, the model will suffer under an unwieldy proliferation of regressors
- => =="overfitting"==

overfitting
- a model has a tight fit to the data it was fed, but at the cost of extrapolation

### Avoiding overfitting

----
$widec$
strict **theory-driven variable selection**  
$result you only include in the model... ($see $down)
$/widec$
----

**1.** ==focal== predictors
- predictors for which you have clear, a priori hypotheses about
_how_ and _why_ they can affect the choice of variant

**2.** ==control== predictors
- factors that are _known_ to affect both the outcome variable and the predictors

$example$
**Control predictor**

- e.g. the age of the participant when
measuring the effect of reading ability (the predictor) on theory-of-mind (the
outcome)

$widec$
$down
$/widec$

- => younger kids will both have lower reading ability and less evolved
theory-of-mind
$/example$

$result corpus-based linguistics
- strict theory-driven variable selection is not feasible, because one does not always have clear-cut a priori hypotheses
- especially ==lexical effects are typically rather open-ended==

### Regularisation methods

regularisation methods
- a family of techniques with Ridge regression, Lasso Regression and Elastic Net as its members
- these techniques introduce a bias, also known as **regularization**
- bias takes the form of a penalty, called ‘lambda’ ($l),
which is multiplied with the coefficient of a (set of) predictor(s)
- => this penalty scaled on the coefficients is then added to the regression equation which is used in the model fitting algorithm.

why make the fit worse with a penalty?
- the penalty makes the estimation of the coefficients more
**==conservative==**
- more **conservative estimates may perform better** when the
model is confronted with **unseen data**
- lower coefficients will **reduce
drastic differences** between the ‘old/seen’ and ‘new/unseen’ data

### Cross-validation

cross-validation
- dividing the available data in a ==training set== and a ==test set==, and to see how well a model performs when confronted with unseen data
- often done in combination
with ==regularization methods== ($see $up)

k-fold cross validation
- used to obtain =='unseen data'==
- the data is repartitioned 𝑘 times
	- $\\1 - \frac{1}{k}$of the
data used as the training set for the model fit
	- $\frac{1}{k}$ of the data used as the test set
- model quality is iteratively checked against the test set

finding $l
- established by minimising the average deviance of the 𝑘 test sets and their
respective 𝑘 training sets
- if all 𝑘 times, the coefficient estimation in the training set gives accurate predictions for the test set, the optimal λ can be kept low.
- if the coefficient estimation in the training set yields a bad
fit for the test set, λ will be higher

$info$
The optimal λ penalty can be so high that
the coefficient is reduced to zero. This means that the variable is not helpful in
predicting the outcome.
$/info$

### Our use of shrinkage with cross-validation

analyses with many potential explanatory variables
- regularisation shrinkage can help us here!
- especially in ==situations where there are more predictors than there are data points==
	- e.g. especially in a lexical effect study

$example$
**Example model: predicting a binary alternating construction**

$widec$
the Dutch dative alternation
$/widec$

- predictors:
	- length of the recipient
	- region (Belgium [0] vs. the Netherlands [1])
- random intercepts:
	- ‘verb’ -> we do not want to assume that _geven_ (‘give’), behaves in
exactly the same way as _vertellen_ (‘tell’), _sturen_ (‘send’), _overhandigen_ (‘hand’) etc. 

$widec$
$down result
$/widec$

$eq$
$widec$
(fitted through a maximum likelihood estimation)
$/widec$

$$
\ln \left(\text { odds }\left(x_{i, j}=\operatorname{PrepDat}\right)\right)=\beta_0+\beta_1 \operatorname{RecLength}_{i, j}+\beta_2 N D_{i, j}+v_i\left(v_i \sim N\left(0, \sigma_i^2\right)\right)
$$

$widec$
$x_{i,j}$: the 𝑗^th^ observation of verb 𝑖,  
$\beta_0$: the model intercept,  
$\beta_1$: the weight for the recipient length (RecLength),  
$\beta_2$: the weight if the observation is from the Netherlands (ND),  
$v_i$: the by-verb correction
$/widec$

$info$
For simplicity’s sake, we will not discuss random slopes here.
$/info$
$/eq$

Such a model, while decidedly better than
a model with fixed effects only, as in $see $down, does not tell us, however, whether
some verbs are more relevant for the model than others.

$eq$
**Fixed-effects model**


$$
\ln \left(\text { odds }\left(x_{i, j}=\operatorname{PrepDat}\right)\right)=\beta_0+\beta_1 \operatorname{RecLength}_{i, j}+\beta_2 N D_{i, j}
$$
$/eq$
$/example$

$eq$
**Lasso model**

$$
\begin{aligned}
&\ln \left(\text { odds }\left(x_j=\operatorname{PrepDat}\right)\right)=\beta_0+\beta_1 \operatorname{RecLength}_j+\beta_2 N D_j+ \color{blue} \sum_{i=1}^n \beta_{3, i} v_{i, j} \color{black} + \\
& \textcolor{red}{\lambda} \left(\left|\beta_1\right|+\left|\beta_2\right|+\sum_{i=1}^n\left|\beta_{3, i}\right|\right)
\end{aligned}
$$

$widec$
𝑛: the number of different verbs,  
𝑖: the verb,  
𝑣~𝑖,𝑗~: the value of the pseudo-observation 𝑗 of verb 𝑖
$/widec$

- we need to slightly transform our data ($see $down)
- we siphon the verbs over to the fixed-effect structure -> <span style="color: blue;">each verb is now entered as a **categorical binary predictor**</span>
- lasso will use a <span style="color: red;">penalty $\lambda$</span> on the absolute value of the coefficient of each of the predictors, see
($see $down)
- the advantage of Lasso
regression over Ridge regression is that it can shrink coefficient(s) all the way
to zero, under an optimal λ
	- coefficients shrunken to zero are not retained by the model, effectively carrying out a ==**variable selection**==
$/eq$

## A real-life example

### Introduction

----
$widec$
*zoeken* <-> *zoeken naar*
$/widec$
----

research question
- you want to investigate whether the alternation is lexically entrenched,
and **depends on the head noun of the theme**
- maybe _paraplu_ (‘umbrella’) prefers the prepositional construction, but another noun, say _kat_ (‘cat’), prefers
the transitive construction
- ==you may lack clear _a priori_ expectations
about how or why certain nouns would be entrenched in the alternating variants==
	- ~= Bloem 2021

avoiding random factors
- one observation of each text from the dataset
- no pronominally realized Themes
- only observations with Theme lemmas that occur at least 10 times

outcome variable
- binary factor (`Variant`)

fixed effects
- complexity of the Theme argument (`Theme Complexity`)
	- calculated as the natural logarithm of its number of words
- position of the Theme argument (`Theme Position`) -> before or after the verb
- Belgium <-> The Netherlands (`Country`)
- head lemma of the Theme argument (`Theme Lemma`)

transforming for Lasso
- `Theme Lemma` transformed to binary factors for each lemma

$info$
 This dataset comprises far too
many variables to run an adequate binomial regression model on the constructional variant. ==By a common rule of thumb, the maximal number of regressors
is $\frac{1}{20}$ of the number of observations of the least frequent outcome level.== In
our case, we have double the number of regressors. 
$/info$

### Throwing the lasso

R library
- `glmnet`

specifics
- 10-fold cross-validated Lasso regression
- binomial regression on all predictors of $see $up
- we arrive at an optimal $l of 0.0007

results
- of the 554 Theme lemmas, the coefficients of 73 are reduced to zero
- of the other covariates, Theme Complexity, Theme Position and Country are
also retained as significant predictors

$info$
A concern might be that the Lasso regularization shrinks Theme lemmas purely on the basis of their frequencies,
but this does not appear to be the case: a binomial regression with a binary
outcome variable Retained by the Lasso, and the `Attested Frequency` of
the `Theme lemma` (i.e. the number of times the `Theme Lemma` occurs in the
dataset) as the predictor, does not reach significance (p = 0.626).
$/info$

### Lexical effects

$gallery$
**Correlation between the Lasso coefficient and the OR-based collostructional
strength (OR) (left) and the Lasso coefficient and the Log-Likelihood-based
collostructional strength (right)**

![Image](img$awuj)

$widec$
Each dot represents a different Theme Lemma. Color coding for whether or not the Theme Lemma is retained by the Lasso.
$/widec$

- the Lasso coefficients show a near-perfect correlation with the OR-based
collostructional strength (Pearson correlation = 0.98, p < 0.001)
- for the Log-Likelihood-based collostructional strength, the correlation is weaker, but
still sizeable (Pearson correlation = 0.52, p < 0.001)
$/gallery$

## Conclusions

$todo