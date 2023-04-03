# 4. Logistic GAMs for Classification

$public=true$

$wide$
from ["Logistic GAMs for Classification" by Noam Ross](https://noamross.github.io/gams-in-r-course/chapter4)
$/wide$

$p=1$

## Types of model outcomes

focus
- using logistic GAMs for binary classification

[ Types of model outcomes ]
|continuous outcomes|binary outcomes|
|---|---|
|- Speed of a motorcycle (mph)|- Presence or absence of an organism in a location|\
|- Fuel efficiency of a car (mpg)|- Whether a purchase was made|\
|- Level of pollution in soil (g/kg)|- Yes/No answer on a survey|

### Probabilities and Log-Odds: Logistic function

#### Modelling in binary

binary outcome
- prediction is a ==probability== -> must be between 0 and 1
- GAM output = *any* number -> not constrained to between 0 and 1
- => convert GAM output to probability using **logistic function**

logistic function
- a transformation that converts numbers of any value to probabilities between zero and one
- the numbers that take on any value can be interpreted as ==log-odds== - the log of the ratio of positive outcomes to negative outcomes

$columns$
$gallery$
**Logistic function**

![Image](img$pjw9)

- turns logit values $-\infty$ to $+\infty$ into probabilities from 0 to 1
$/gallery$

$gallery$
**Logit function**

![Image](img$5jrr)

- turns probabilities from 0 to 1 into logit values from $-\infty$ to $+\infty$
$/gallery$
$/columns$

#### Logistic and Logit Functions in R

$columns$
$code$
**Logistic function**

```r
plogis(-2.5)
```

$widec$
$down
$/widec$

```r
[1] 0.07585818
```
$/code$

$code$
**Logit function**

```r
qlogis(0.75)
```

$widec$
$down
$/widec$

```r
[1] 1.098612
```
$/code$
$/columns$

### Logistic GAMs with `mgcv`

`family=binomial`
- adds the logit link

$code$
```r
gam(y ~ x1 + s(x2),
   data = dat,
   family = binomial,
   method = "REML")
```
$/code$

$code$
**Output**

```r
Family: binomial 
Link function: logit 

Formula:
y ~ s(x1) + s(x2)

Parametric coefficients:
            Estimate Std. Error z value Pr(>|z|)    
(Intercept)   0.7330     0.1208    6.07 1.28e-09 ***
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Approximate significance of smooth terms:
        edf Ref.df Chi.sq  p-value    
s(x1) 1.367  1.646  25.83 1.23e-05 ***
s(x2) 5.754  6.890  51.37 8.12e-09 ***
```

- output of logistic GAM is similar to that of our previous GAMs
	- non-parametric terms on top
	- smooths on the bottom
	- EDFs still indicate the complexity of the smooths
	- asterisks indicate the significance

$warn$
**Logit scale warning**

The outputs in the `summary()` function are on the **log-odds scale**!  To interpret them as probabilities, we need to convert them using the logistic function.

```r
plogis(0.7330) # Intercept
```

$widec$
$down
$/widec$

```r
[1] 0.6754633
```

- $result Means that the model predicts a ==67 percent baseline chance of a positive outcome==. This is what we would expect if x1 and x2 were at their average values.
$/warn$
$/code$

### Example data set

$code$
**`csale` data set**

```r
  purchase n_acts bal_crdt_ratio avg_prem_balance retail_crdt_ratio
1        0     11        0.00000         2494.414           0.00000
2        0      0       36.09506         2494.414          11.49123
3        0      6       17.60000         2494.414           0.00000
4        0      8       12.50000         2494.414           0.80000
5        0      8       59.10000         2494.414          20.80000
6        0      1       90.10000         2494.414          11.49123
  avg_fin_balance mortgage_age cred_limit
1        1767.197     182.0000      12500
2        1767.197     138.9601          0
3           0.000     138.9601          0
4        1021.000     138.9601          0
5         797.000      93.0000          0
6        4953.000     138.9601          0
```

- anonymised information from the insurance industry
- outcome, "purchase", represents whether customers purchased insurance following a direct mail campaign
- other variables consist of information from credit reports of those customers
$/code$

$p=4$

## Visualising logistic GAMs

### Log-odds plots

scale
- log-odd scale
- can be difficult to interpret
- we understand the _shape_ of the effect, but the _magnitude_ of the effect on probability is not immediately apparent

$code$
```r
plot(binon_mod)
```

$gallery$
![Image](img$xmrw)
$/gallery$

$/code$

### Transformed plots

transformation
- `trans=plogis`
- converts our output to the probability scale

$code$
```r
plot(binon_mod, pages = 1, trans = plogis)
```

$gallery$
![Image](img$3qy5)

- outputs are now on a ==probability scale between zero and one==. 
- the first term, which was previously linear, has a slight curve on this scale
$/gallery$
$/code$

### Intercepts

#### Adding intercept

$acco$
default transformation
- partial effects are all centred on an average value of 0.5
- => we are looking at ==each partial effect with no intercept==

$gallery$
![Image](img$jiai)
$/gallery$
$/acco$

$widec$
$down
$/widec$

$acco$
adding an intercept
- we `shift` the plot by `coef(binom_mod)[1]` (= the intercept)

$code$
```r
plot(binom_mod, pages = 1, trans = plogis,
     shift = coef(binom_mod)[1])
```

$gallery$
![Image](img$so1j)
$/gallery$

$info$
Now our partial effect plots have ==much more intuitive interpretation==. Each partial effect plot can be interpreted as ==showing the probability of the outcome if all other variables were at their average value==. At their own average value, you get only the effect of the intercept.

$gallery$
![Image](img$isug)
$/gallery$
$/info$
$/code$
$/acco$

#### Incorporating intercept uncertainty

`seWithMean`
- adds the _intercept_ uncertainty to the _smooth_ uncertainty
- natural to include this uncertainty here, as we are adding the intercept term

$code$
```r
plot(binom_mod, pages = 1, trans = plogis,
     shift = coef(binom_mod)[1],
     seWithMean = TRUE)
```

$gallery$
![Image](img$6cs1)

- the confidence intervals in our partial effect plots now also have a natural interpretation
- may be interpreted as ==the range of uncertainty of the probability of the outcome for any value of the variable, holding other variables equal at their average value==
$/gallery$
$/code$

### Improving the plot

$code$
```r
plot(binom_mod, pages = 1, trans = plogis, shift = coef(binom_mod)[1],
     seWithMean = TRUE, rug = FALSE, shade = TRUE, shade.col = "lightgreen", 
     col = "purple")
```

$gallery$
![Image](img$c5iw)
$/gallery$
$/code$

$p=9$

## Making predictions

### `predict()` function

#### Using the `predict` function

$code$
```r
predict(log_mod2)
```

$widec$
$down
$/widec$

```r
            1             2             3             4 
-0.8672827973 -2.9135420237 -0.4839780158 -0.1996086132 
            5             6             7             8 
-0.4416783066 -1.2351679544 -0.6148559122 -2.9135420237 
...
```

- simply running predict() on a model (in this case our logistic model of purchasing behavior) will yield a vector of predictions for each data point in the data set we used to fit the model
$/code$

#### Transforming predictions

$code$
**Link scale (logit scale)**

```r
predict(log_mod2, type = "link")
```

$widec$
$down
$/widec$

```r
            1             2             3             4 
-0.8672827973 -2.9135420237 -0.4839780158 -0.1996086132 
            5             6             7             8 
-0.4416783066 -1.2351679544 -0.6148559122 -2.9135420237 
...
```
$/code$

$code$
**Probability scale**

```r
predict(log_mod2, type="response")
```

$widec$
$down
$/widec$

```r
         1          2          3          4 
0.29582001 0.05148818 0.38131322 0.45026288 
         5          6          7          8 
0.39134114 0.22527819 0.35095230 0.05148818 
...
```
$/code$

#### Including standard errors

$code$
```r
predict(log_mod2, type = "link", se.fit = TRUE)
```

$widec$
$down
$/widec$

```r
$fit
         1          2          3          4 
-0.8672828 -2.9135420 -0.4839780 -0.1996086 
         5          6          7          8 
-0.4416783 -1.2351680 -0.6148559 -2.9135420 

$se.fit
        1         2         3         4 
0.2850848 0.1646090 0.2299404 0.2159088 
        5         6         7         8 
0.2767443 0.7601131 0.2454877 0.1646090 
```

`$se.fit` contains standard errors for our predictions.
$/code$

$warn$
Standard errors are only approximations when we use the probability scale. This is because ==errors are non-symmetrical on this scale==. If you use standard errors to construct confidence intervals for your predictions, you should do so on the ==log-odds scale, and _then_ convert them to probability using the plogis() logistic function==.

$gallery$
![Image](img$exyh)

- **left:** what happens when we make intervals by adding or subtracting on the response scale
	- predictions of probability can be below zero or above one -> doesn't make any sense
- **right:** correctly made intervals (transformed after addition)
$/gallery$
$/warn$

### Predictions on new data

$code$
**Build models**

```r
trained_model <- gam(response ~ s(predictor),
                     data = train_df,
                     family = binomial,
                     method = "REML")
```

$widec$
$down
$/widec$

```r
# Test data
test_predictions <- predict(trained_model,
                            type = "response",
                            newdata = test_df) 
```
$/code$

#### Explaining predictions by terms

explanation by terms
- how much does each term contribute to an individual prediction?
- set the `type` argument to `"terms"` in `predict()`
- => produces a matrix showing the contribution of each smooth to each prediction

$code$
```r
predict(log_mod2, type = "terms")
```

$widec$
$down
$/widec$

```r
      s(n_acts) s(bal_crdt_ratio) s(avg_prem_balance)
1     1.2115213      0.3327855673        -0.135920526
2    -0.8850186     -0.4058818961        -0.135920526
3     0.5693622      0.2972364048        -0.135920526
4     0.8974704      0.3827671103        -0.135920526
5     0.8974704     -0.0727464938        -0.135920526
6    -0.6228781      0.1936974771        -0.135920526
7     0.3642246      0.3377181800        -0.135920526
8    -0.8850186     -0.4058818961        -0.135920526
9     1.0209905      0.3604064595         0.317309246
10    1.7675666     -0.4533384774         0.346837355
      s(retail_crdt_ratio) s(avg_fin_balance) s(mortgage_age) s(cred_limit)
              0.0678994892       -0.040572487   -0.2918390343    -0.3705562
             -0.0075325272       -0.040572487   -0.0209184283     0.2229033
              0.0678994892        0.156060412   -0.0209184283     0.2229033
              0.0626482277        0.032042157   -0.0209184283     0.2229033
             -0.0686510698        0.065216680    0.2906502573     0.2229033
             -0.0075325272        0.776081676   -0.0209184283     0.2229033
             -0.0075325272       -0.040572487    0.2849244308     0.2229033
             -0.0075325272       -0.040572487   -0.0209184283     0.2229033
             -0.0253158695       -0.040572487    0.3551533139    -0.3230903
              0.0377046376        0.150927175    0.1269458733    -0.5366686
```

$info$
If we were to sum across all the columns of this matrix, and add the intercept, we would have our overall prediction the log-odds scale.
$/info$
$/code$

$example$
**Row 1**

```r
predict(log_mod2, type = "terms")[1, ]
```

$widec$
$down
$/widec$

```r
           s(n_acts)    s(bal_crdt_ratio) 
          1.21152126           0.33278557 
 s(avg_prem_balance) s(retail_crdt_ratio) 
         -0.13592053           0.06789949 
  s(avg_fin_balance)      s(mortgage_age) 
         -0.04057249          -0.29183903 
       s(cred_limit) 
         -0.37055621 
```

- for this one data point, the number of accounts has about four times the effect in increasing purchase probability prediction than balance-credit ratio
- mortgage age and credit limit influence the prediction in the opposite direction, about the same amount as balance-credit ratio
- if we add these terms up, add the intercept, and transform using the `plogis()` function, we get this data point's predicted purchase probability
$/example$