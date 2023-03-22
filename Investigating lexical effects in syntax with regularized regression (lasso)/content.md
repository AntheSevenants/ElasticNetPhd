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

$p=166$

## Introduction

### Influence of the lexicon on form

usage-based construction grammar
- no clear distinction between lexicon and syntax
- "It's constructions all the way down" (Goldberg 2006)
- => argument constructions and syntactic frames have their own meaning

$p=166-167$

$example$
**Example of constructions with overlapping meanings (dative alternance)**

- double object construction
- prepositional object construction

$wide$
- $result can both express transfer of possession, and can occur with verbs
like _give_, _donate_, _pass_, _transfer_, _send_ ...
- $result the choice is driven ==multifactorially== (Bresnan et al., 2007 and the references mentioned above), by **animacy** and **topicality** among other factors
- /!\ at the same time: not all verbs that engage in this argument construction choice have the same weights for these factors => ==some verbs are more sensitive to these factors than others==
$/wide$
- $result **some verbs may have a very strong predilection for one of the variants**
$/example$

$p=167$

verb meaning (in usage-based construction grammar)
- does not determine ==_by itself_== the construction it combines with (<-> lexicalist/projectionist approaches)
- has to do with ==frequency of use, entrenchment, analogy==
and partially also with ==<u>lexical semantics</u>==
	- (see Perek, 2015; Diessel, 2019: Ch. 7; Pijpops, 2019)
- => many different factors contribute to the choice of variant

### Investigating the influence

how to investigate?
- **integrate** the ==lexical semantic effects== into the multifactorial accounts
- e.g. linear regression (mostly with the logit link)

$info$
**More info about linear regression**

See Gries, 2000; Grondelaers, 2000 for pioneering work,
and Speelman, 2014 for a good, short introduction.
$/info$

why linear regression?
- can deal with a multifactorial design combining both **extralinguistic**
predictors (such as age, gender, and socio-economic status) and **intralinguistic predictors**
- gives both **effect size** and **significance**
- can simultaneously integrate **numeric** and
**categorical** variables, and **interaction effects**
- => models achieve ==high levels of explained variance, as measured by $R^2$==

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

$p=168$

**2.** building the statistical model for one verb only
- e.g. in the dative alternation, one could look solely at the prototypical example of _give_ in its transfer of possession sense
- 👍 advantage: the lexical effect is kept under tight control
- 👎 disadvantage: generalizability: the result is ==robust==, but ==cannot be straightforwardly extrapolated to
other verbs==

**3.** building a mixed-effect model with ==different intercepts (&/slopes) for different verbs==
- state-of-the-art solution in corpus linguistics today (Gries, 2015)
- 'random effects' -> necessary to be conservative about the effect of the focus predictors in the fixed effects
	- part of the variation can be attributed to random effects, and we make sure not to lump other variation together with our main effects
- 👍 advantage: great for avoiding Type I errors
- 👍 advantage: only adds one term to the model
- 👎 disadvantage: increase in Type II errors:
the amount of variance that is left to be explained by the fixed effects
may be too small to distinguish subtle effects
	 - this may be the case if
there is ==collinearity between the effects of different verbs and the focal
variables== (since ==variation due to focal variable may be attributed to verbal random effect==)
	- => the coefficients for the predictors are partialled out over different lexemes that occur in the construction -> obscures the precise lexical
effects

$widec$
$down

if you are really interested in the effects of the individual verbs, ==_why are they not part of the fixed-effect structure_?==
$/widec$

$info$
**Maximal model supremacy**

Corpus linguistics -- to the extent that they have adopted
the mixed-model approach -- also tend to follow the ‘keep it maximal’
credo, adding random slopes to their models, even in cases where this
is not fully warranted, further reducing the power of the fixed effects
(see Winter, 2020: 242). 
$/info$

$p=169$

**4.** binning verbs
- creating large categories of verbs (especially for lower frequency verbs)
- 👍 advantage: limited number of levels for main effect predictor
- 👎 disadvantage: underpopulated words all considered to be the same
- 👎 disadvantage: misrepresentation of non-independence of observations
	- flouts the very motivation of random effects

### Other possibilities

**1.** memory-based learning
- 👍 advantage: one no longer assumes a stable effect of higher-order notions
	- e.g. animacy, topicality
	- often too vague to be falsifiable and are difficult to operationalize (Dąbrowska 2017: 23–25, 37)
- 👎 disadvantage: 'black-box'

$info$
For examples of memory-based learning, see Daelemans and van den Bosch (2005);
Theijssen et al. (2013); Van den Bosch and Bresnan (2015); Pijpops (2019) and
De Troij et al. (2021).
$/info$

**2.** (Distinctive) Collexeme Analysis
- 👍 advantage: clear view on lexical effects
- 👎 disadvantage: do not allow for multifactorial control (Bloem, 2021: 115)
- 👎 disadvantage: may be prone to overfitting

### Using Lasso regression with cross-validation

$acco$
----
$widec$
👍 Advantages
$/widec$
----

1. we can stick to the **regular linear regression design**
2. we retain **multivariate control** (with extra-linguistic and intra-linguistic factors) and interactions
3. we get a **generalisable method** that is more **robust against overfitting**
4. we are **not stretching the use of random effects** beyond what they are designed for
5. we are able to **increase the number of predictors** that can be entered into the regression
$/acco$

$p=169-170$

$acco$
----
$widec$
👎 Disadvantages
$/widec$
----

1. we have to give up on the mixed-model design for *other* typical random factors
	- e.g. author, text, genre
	- solution $1: if the genre division is not too
fine-grained, it may be wiser to enter it as a fixed effect
	- solution $2: avoid using multiple observations from the
same text file

$info$
In synchronic corpus
linguistics, with increasingly large corpora, often in the order of magnitude of
hundreds of millions or even of billions of tokens, you can probably just ignore this. 
$/info$

$info$
Pioneering papers like Bondell
et al. (2010), Schelldorfer et al. (2011), and Groll and Tutz (2014) show that
advances are made to integrate random effects in penalized regularization.
$/info$
$/acco$

$p=170$

## Regularised Regression

### Overfitting

entering lemmas as fixed effects
- often not a sensible option in corpus studies for lexical effects
- if the lemma is treated as a factor with all lexemes as
factor levels, the model will suffer under an unwieldy proliferation of regressors
- => =="overfitting"==

overfitting
- a model has a tight fit to the data it was fed, but at the cost of extrapolation / generalisation

$p=171$

### Avoiding overfitting

----
$widec$
strict **theory-driven variable selection** -> to ==avoid overfitting==!  
$result you only include in the model... ($see $down)
$/widec$
----

$acco$
**1.** ==focal== predictors
- predictors for which you have clear, a priori hypotheses about
_how_ and _why_ they can affect the choice of variant
$/acco$

$acco$
**2.** ==control== predictors
- factors that are _known_ to affect both the outcome variable and the predictors

$example$
**control predictor**

- e.g. the age of the participant when
measuring the effect of reading ability (the predictor) on theory-of-mind (the
outcome)

$widec$
$down
$/widec$

- => younger kids will both have lower reading ability and less evolved
theory-of-mind
$/example$
$/acco$

$result /!\ corpus-based linguistics
- strict theory-driven variable selection is not feasible, because one does not always have clear-cut a priori hypotheses
- especially ==lexical effects are typically rather open-ended==

### Regularisation methods

regularisation methods
- a family of techniques with Ridge regression, Lasso Regression and Elastic Net as its members
- these techniques introduce a bias, also known as **regularization**
- bias takes the form of a ==penalty==, called ‘lambda’ ($l),
which is ==multiplied with the coefficient(s)== of a (set of) predictor(s)
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

$p=172$

### Our use of shrinkage with cross-validation

analyses with ==many potential explanatory variables==
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
\begin{aligned}
& \ln \left(\operatorname{odds} \left(x_{i, j}= \operatorname{PrepDat}\right)\right) = \\ 
& \beta_0+\beta_1 \cdot \operatorname{RecLength}_{i, j}+\beta_2 \cdot \text{ND}_{i, j}+v_i \\
&\left(v_i \sim N\left(0, \sigma_i^2\right)\right)
\end{aligned}
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
\ln \left(\operatorname{odds}\left(x_{i, j}=\operatorname{PrepDat}\right)\right)= \color{green} \beta_0 + \beta_1 \cdot \operatorname{RecLength}_{i, j}+\beta_2 \cdot \text{ND}_{i, j}
$$
$/eq$
$/example$

$eq$
**Lasso model**

$$
\begin{aligned}
&\ln \left(\operatorname{odds}\left(x_j=\operatorname{PrepDat}\right)\right)= \\
& \color{green} \beta_0+\beta_1 \cdot \operatorname{RecLength}_j+\beta_2 \cdot \text{ND}_j \color{black} + \color{blue} \sum_{i=1}^n \beta_{3, i} \cdot v_{i, j} \color{black} + \\
& \color{red} {\lambda} \left(\left|\beta_1\right|+\left|\beta_2\right|+\sum_{i=1}^n\left|\beta_{3, i}\right|\right)
\end{aligned}
$$

$widec$
<span style="color: green;">common part from fixed effect regression ($see $up)</span>  
𝑛: the number of different verbs,  
𝑖: the verb,  
𝑣~𝑖,𝑗~: the value of the pseudo-observation 𝑗 of verb 𝑖
$/widec$

- we need to slightly transform our data to accord to new equation structure
- we siphon the verbs over to the fixed-effect structure -> <span style="color: blue;">each verb is now entered as a **categorical binary predictor**</span>
- lasso will use a <span style="color: red;">penalty $\lambda$</span> on the absolute value of the coefficient of each of the predictors, see
($see $down)
- the advantage of Lasso
regression over Ridge regression is that it can shrink coefficient(s) all the way
to zero, under an optimal λ
	- coefficients shrunken to zero are not retained by the model, effectively carrying out a ==**variable selection**==
$/eq$

$p=173$

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
	- nvda. ~= Bloem 2021

### Methodology

dataset
- from Pijpops (2019), based on the SoNaR corpus

avoiding random factors
- one observation of each text from the dataset
- no pronominally realized Themes
- ==only observations with Theme lemmas that occur at least 10 times==

outcome variable
- binary factor (`Variant`) -> either DO/PO

$p=173-174$

fixed effects
- complexity of the Theme argument (`Theme Complexity`)
	- calculated as the natural logarithm of its number of words
- position of the Theme argument (`Theme Position`) -> before or after the verb
- Belgium <-> The Netherlands (`Country`)
- head lemma of the Theme argument (`Theme Lemma`)

$p=174$

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
- of the 554 `Theme` lemmas, the coefficients of 73 are reduced to zero
- of the other covariates, `Theme Complexity`, `Theme Position` and `Country` are
also retained as significant predictors

$info$
A concern might be that the Lasso regularization shrinks Theme lemmas purely on the basis of their frequencies,
but this does not appear to be the case: a binomial regression with a binary
outcome variable Retained by the Lasso, and the `Attested Frequency` of
the `Theme lemma` (i.e. the number of times the `Theme Lemma` occurs in the
dataset) as the predictor, does not reach significance (p = 0.626).
$/info$

### Lexical effects

#### Comparison to Distinctive Collexeme Analysis (theoretical)

|Lasso regression|Distinctive Collexeme Analysis|
|---|---|
|coefficients obtained under ==multivariate control==|pulls ==*not*== obtained under ==multivariate control==|
|regression coefficients|- ==Log Likelihood Measure==|\
| |	- sensitive to frequency, uses significance values|\
| |- ==Odds Ratio (OR)==|\
| |	- frequency insensitive, uses effect size|\
| |	- ranges from $-\infty$ to $+\infty$|\
| |	- can be directly compared to the coefficients of $see <-|

$p=175$

#### Comparison to Distinctive Collexeme Analysis (results)

$gallery$
**Correlation between the Lasso coefficient and the OR-based collostructional
strength (OR) (left) and the Lasso coefficient and the Log-Likelihood-based
collostructional strength (right)**

![Image](img$awuj)

$widec$
Each dot represents a different Theme Lemma. Color coding for whether or not the Theme Lemma is retained by the Lasso.
$/widec$

|OR-based collostructional strength|Log-Likelihood-based collostructional strength|
|---|---|
|==near-perfect correlation==|==weaker==, but ==still sizeable==|
|Pearson correlation = 0.98, p < 0.001|Pearson correlation = 0.52, p < 0.001|
$/gallery$

$wide$
- $result Lassy Regression seems to do what it's supposed to
$/wide$

#### Explaining the attraction

----
$widec$
What determines the choice for either constructional variant at a lexical level?
$/widec$
----

$p=176$

$acco$
**1.** 'birds of a feather flock together'
- some theme lemmas are semantically close to one another
- => those form a cluster with predilection for the same variant

$info$
it is certainly not a strict law, but merely a (possibly hard to
eyeball) tendency, as near-synonyms can be attracted to different variants (see
also diessel, 2019, ch.7 and gries and stefanowitsch, 2004).
$/info$
$/acco$

$acco$
**2.** distributional semantics
- semantic vectors for all Theme lemmas taken from [Snaut](http://meshugga.ugent.be/snaut-dutch/)
- vectors used to build a matrix of cosine distances

$result visualisation
- cosine distance matrix then turned into a ==dendogram== (using Ward clustering method)
- then, we made 457 cluster groupings with increasing granularity
	- from a macro-cluster with ==two groups== to a micro-cluster with ==458 groups==
	- in this last group, all `Theme Lemma`s are in their own cluster

$info$
Not all Theme Lemmas are
represented in the repository, but 95% (458 out of the 481) are.
$/info$

$result regression analysis
- for each of the clusters, we ran **3 regression analyses**
	1. cluster membership ~ ==Lasso coefficient==
	2. cluster membership ~ ==OR-based collostructional strength==
	3. cluster membership ~ ==Log-Likelihood-based collostructional strength==
- rationale: choice for an object = partially determined by group membership in the cluster
- for all 1,371 (457 * 3) regression analyses, we extracted the R² value
	- obviously, the last 458-groups cluster will have a perfect R² for both regression
analyses

$gallery$
**Results**

$widec$
![Image](img$odpx)
$/widec$

$widec$
(image = p. 177)
$/widec$

$wide$
- **x-axis:** number of groups in the hierarchical cluster
- **y-axis:** $R^2$ value for the three methods
$/wide$

$wide$
- ==Lasso methods progresses in lock-step with the OR-based Distinctive Collexeme Analysis==
- both consistently outperform Log-Likelihood-based Distinctive Collexeme Analysis
$/wide$

$info$
See Levshina and Heylen (2014) and Pijpops (2019) for a related
approach.
$/info$
$/gallery$
$/acco$

$widec$
$down main take-aways
$/widec$

1. ==Lasso Regression performs adequately==
2. effect-size-based Collostructional methods are preferable over
significance-based Collostructional methods

$p=177$

## Conclusions

Lassy regression
- regularisation technique from the field of machine learning
- can be used for assessing ==lexical effects in syntax!==
- 👍 advantage: works under multivariate control
- 👍 advantage: computationally relatively light
- 👎 disadvantage: does not take into account random effects

$p=177-178$

$info$
Lasso regression is currently extended to mixed models as well
(e.g. Groll and Tutz, 2014), and users of the R software may fruitfully apply the
`glmmLasso` package for R (Groll, 2017). The mathematics are more complex, the method is computationally much heavier, and finding the optimal lambda penalty is not as straightforward. 
$/info$

$p=178$

fixed-effect Lasso regression
- strikes a reasonable balance between complexity and usability