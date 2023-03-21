$public=true$

# Changing Preferences in Cultural References

$reader$
**Abstract**

This proof-of-concept paper advocates the use of lasso regularization
for tracking changes in our collective cultural memory, as documented in the
journal _De Gids_, issues 1837–1999. Mentions of 264 well-known historical figures
are treated as predictors in an aggregate binomial regression for the era of publication, to see which historical figures are more likely to be mentioned in issues
in the (long) nineteenth century, as opposed to the (short) twentieth century.
$/reader$

## Introduction

language
- not used in an abstract logical space
- grounded in **cultural context**

cultural context
- impacts lexicon, syntax, pragmatics ...
- connected with anthropology, archaeology, cultural history

problem of cultural context
- open-ended, fuzzy concept
- hard to operationalise!
- <-> social variables: age, socio-economic status

prime data for investigation
- text corpora (historical corpora)
- testify to what was ==collectively on people’s minds at
the time of writing==

this study
- context of the cultural canon and our changing preference
- Which historical figures have featured prominently in our recent past? Which have fallen out of favour?

method
- **lasso regression**

## Methods

### Cross-validation

cross-validation
- dividing the available data in a training set and a test set, and to see how well a model performs when confronted with unseen data
- often done in combination
with so-called regularization methods

regularisation methods
- a family of techniques with Ridge regression, Lasso Regression and Elastic Net as its members
- these techniques introduce a bias, also known as **regularization**
- bias takes the form of a penalty, called ‘lambda’ ($l),
which is multiplied with the coefficients of a (set of) predictor(s)
- => this penalty scaled on the coefficients is then added to the regression equation which is used in the model fitting algorithm.

### Lasso regression

why make the fit worse with a penalty?
- the penalty makes the estimation of the coefficients more
**conservative**
- more **conservative estimates may perform better** when the
model is confronted with **unseen data**
- lower coefficients will **reduce
drastic differences** between the ‘old/seen’ and ‘new/unseen’ data

k-fold cross validation
- used to obtain 'unseen data'
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

### Difference between ridge regression and lasso regression

|ridge regression|lasso regression|
|---|---|
|multiplies lambda with the square of the coefficients|uses the absolute value of the coefficients|
|coefficients will never reach zero|can shrink coefficients all the way to zero (under an optimal lambda)|

### Equation

$eq$
$$
\operatorname{logit}\left(y_j\right)=\beta_0+\sum_{i=1}^n \beta_i x_{i j}+\lambda\left(\alpha \sum_{i=1}^n\left|\beta_i\right|+(1-\alpha) \sum_{i=1}^n \beta_i^2\right)
$$

$widec$
𝑛: number of predictors,  
𝑥~j~: value of the observation for variable 𝑖,  
α: between 0 and 1, balances between lasso and ridge  
$result closer to 1 -> more lasso, closer to 0 -> more ridge
$/widec$
$/eq$

shrinkage
- conservative estimation of the regression coefficients by shrinking them
- particularly useful in analyses with many potential explanatory variables
- you can have more variables than you have observations
	- usually impossible to fit a hyperplane, but using cross-validation we can do this

$reader$
When we are dealing with a large number of predictors, some of which could
have no effect on the outcome variable, lasso is generally preferred over ridge,
because it can easily jettison irrelevant predictors by shrinking the coefficients to
zero. In other words: lasso does variable selection. When there is high collinearity
between the variables, elastic net may be preferred, because it is less ruthless in
selecting variables to throw them out, but the downside is that the variable selecting power is reduced somewhat.

$widec$
(p. 588)
$/widec$
$/reader$

proof-of-concept article
- we use simple lasso regression
- => ɑ = 1 in $see $up

### Implementation

"occurrences"
- flattened to simply one occurrence
- => binary predictor -> either the historical figure is mentioned in a particular text or they are not mentioned

model output
- regularised coefficients on the logit scale for each historical figure
- if we add the model intercept and then put the result through the logistic function (the inverse of the logit), we have an estimation for the attraction towards a period

$eq$
**Attraction function for historical figures**

$bigtex$
$$
\operatorname{Att}\left(h_i\right) = \frac{1}{1 + e^{-(\beta_0 + \beta_{h_i})}}
$$
$/bigtex$

$widec$
ℎ~𝑖~: specific historical figure,  
𝐴𝑡𝑡: attraction towards the twentieth century  
intercept: estimate of the era in the absence of any predictors
$/widec$
$/eq$

## Results

### Technical details

optimal lambda
- $\\0.004$

intercept
- $\\\\0.213$ (logit)
- “on average”, historical figures are estimated to have a slight pull towards the twentieth century

keep/delete figures
- 187 historical figures have been retained in the lasso
- 77 historical figures are thrown out ($see $down)

$reader wide$
**Deleted authors**

- Adriaen de Vries (3)
- Alcibiades (45)
- Alexander Farnese (25)
- Alfred de Grote (1)
- Anders Celcius (37)
- Anna van Buren (5)
- Anna van Saksen (22)
- Archimedes (75)
- Assoerbanipal (7)
- Augustinus (280)
- Averroës (16)
- Caspar Luyken (33)
- Childerik (4)
- Christoffel Columbus (179)
- Claudius Galenus (43)
- Dante Alighieri (517)
- Democritus (42)
- Elizabeth Stuart (2)
- François Rabelais (141),
- Georg Friedrich Händel (1)
- Gerard David (19)
- Giovanni Boccaccio (81),
- Gnaius Julius Agricola (9)
- Hannibal Barkas (69)
- Hans Memling (16)
- Hendrik IV van Frankrijk (2)
- Hubert van Eyck (1)
- Hugo van der Goes (10)
- Innocentius III (31)
- Isabella I van Castilië (7)
- Jacob Kats (7)
- Jacob van Heemskerck (21),
- Jakob Fugger (6)
- Jan van Goyen (16)
- Jan Zonder Land (2)
- Jean Froissart (18),
- Joachim Patinir (4)
- Johannes Calvijn (182)
- Joost van den Vondel (55)
- Karel de Goede (2)
- Karel III van Spanje (2)
- Karel VI de Waanzinnige (1)
- Leonardo
- Da Vinci (1)
- Lodewijk van Nassau (36)
- Maarten van Rossum (4)
- Marco Polo (30)
- Marcus Aurelius (79)
- Margaretha van Beieren (1)
- Maria van Gelre (2),
- Marie Antoinette (47)
- Michel de Montaigne (4)
- Nicolaus Copernicus (108),
- Peter Abelard (7)
- Peter Stuyvesant (5)
- Philip Melanchthon (29)
- Philips van Marnix (34)
- Piet Hein (44)
- Pieter van Gent (1)
- Plato (684)
- Plutarchus (160),
- Polybios (31)
- Polykrates (11)
- Rembert Dodoens (18)
- Rembrandt (531)
- Robert Campin (2)
- Rogier van der Weyden (14)
- Stefan Lochner (2)
- Thales (48),
- Thomas Hobbes (69)
- Thucydides (105)
- Urbanus II (4)
- Vercingetorix (4),
- Willem de Veroveraar (13)
- William Penn (10)
- William Shakespeare (967),
- William Wallace (1)
- Xenophanes (24)

It is not the case that the lasso shrinks only very infrequently mentioned historical figures. There appears to be neither a correlation between the lasso coefficient
and the frequency of mention (Pearson correlation -0.11, p = 0.146), nor between
retention by the lasso (yes/no) and the frequency of mention (OR 1.00, p = 0.469).
Nonetheless, some caution is evidently to be observed for estimates of historical
figures that have a low frequency.
$/reader wide$

$gallery port$
$widec$
![Image](img$kj05)

![Image](img$ml4o)

![Image](img$0nf2)

![Image](img$jfn4)

![Image](img$6rcz)

![Image](img$irdy)

![Image](img$7ldr)

![Image](img$h41s)
$/widec$

$/gallery port$

### Trends

----
$widec$
Is there a difference between political figures, religious leaders, artists, or scientists?
$/widec$
----

weighted linear regression
- estimates of the lasso regression as a response variable
- predictors
	- domain
	- frequency of mentions -> give more weight to frequently mentioned figures

$gallery port$
$widec$
![Image](img$lwyc)
$/widec$

$widec$
artists = reference level
$/widec$

- scientists have a stronger pull towards recent issues (p = 0.024)
- political leaders are more likely to be mentioned in earlier issues (p < 0.001)
- for religious leaders, significance was not reached (p = 0.241)
$/gallery port$

## Further applications

use of lasso regression
- very useful when the number of predictors is large