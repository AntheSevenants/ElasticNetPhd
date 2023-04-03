# 3. Spatial GAMs and Interactions

$public=true$

$wide$
from ["Spatial GAMs and Interactions" by Noam Ross](https://noamross.github.io/gams-in-r-course/chapter3)
$/wide$

$p=1$

## Two-dimensional smooths and spatial data

focus
- smooths of multiple variables
- interactions
- => will allow us to look at new kinds of data, especially geospatial data

### Interactions in linear models

$eq$
$bigtex$
$$
y = \beta_1 \cdot x_1 + \beta_2 \cdot x_2 + \color{red} \beta_3 \cdot x_1 \cdot x_2
$$
$/bigtex$
$/eq$

interactions in linear modelling
- represent the fact that outcomes depend on ==non-independent relationships of multiple variables==
- generally represented by adding a term multiplying two variables
	- can result in the ==outcome being higher or lower than what would be predicted by the sum of the two values alone==

### Interactions in GAMs

interactions in GAMs
- relationship between a variable and an outcome ==changes across the range of the smooth==
- => represent interactions between variables as a ==smooth surface==

$eq$
$bigtex$
$$
y = s(x_1, x_2)
$$
$/bigtex$

$gallery$
![Image](img$5jqb)
$/gallery$
$/eq$

#### Modelling interactions in R

modelling interations
- put two variable sinside `s()` function

$code$
```r
gam(y ~ s(x1, x2), # <- 2 variables 
        data = dat, method = "REML")
```
$/code$

#### Mixing interaction and single terms

interaction and single terms
- can co-exist
- you can include discrete, categorical terms along with interactions and linear terms.

$code$
```r
gam(y ~ s(x1, x2) + s(x3),
        data = dat, method = "REML")
```

```r
gam(y ~ s(x1, x2) + x3 + x4,
        data = dat, method = "REML")
```
$/code$

$info$
A common way to model geospatial data is to use an interaction term of x and y coordinates, along with individual terms for other predictors. ==The interaction term then accounts for the spatial structure of the data.==
$/info$

#### Interaction model outputs

$code$
```r
summary(model_with_interactions)
```

$widec$
$down
$/widec$

```r
Family: gaussian 
Link function: identity 
 
Formula:
y ~ s(x1, x2)
 
Parametric coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  0.34256    0.01646   20.82   <2e-16 ***
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
Approximate significance of smooth terms:
           edf Ref.df     F p-value    
s(x1,x2) 10.82   14.9 14.37  <2e-16 *** # <- Interaction
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
R-sq.(adj) =  0.519   Deviance explained = 54.5%
GCV = 0.057564  Scale est. = 0.054161  n = 200
```

- the interaction is a single smooth term -> ==combines the effects of x1, x2, _and their combination_ in a single smooth==
	- differs from what you may expect in a linear model, where terms for x1, x2, and their combination are _separate_
- note the high EDF for this term
	- it takes many more basis functions, and therefore more data, to build a two-dimensional surface rather than a one-dimensional line
$/code$

### Example dataset

$code$
**Meuse dataset**

```r
meuse
```

$widec$
$down
$/widec$

- geospatial data of heavy metal soil pollution along the Meuse river in the Netherlands
- consists of a data frame with
	1. x and y coordinates
	2. measures of heavy metals in the soil
	3. other spatial covariates such as elevation, distance from the river, and the land-use type occurring in that location
$/code$

$p=4$

## Plotting and interpreting GAM interactions

complexity of GAM interactions
- challenging!
- solution: visualisation!

### 2D `plot()`

contour plot
- represents both variables and their interaction
- interior = a topographic map of predicted values
- contour lines = ==points of equal predicted values== -> labelled
- dotted lines = uncertainty in prediction
	- how contour lines would move if predictions were one standard error higher or lower

$code$
```r
plot(mod_2d)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$3mxg)
$/gallery$
$/code$

### 3D `plot()`

3D perspective plot
- `plot()` with `scheme=1`

$code$
```r
plot(mod_2d, scheme=1)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$f3f8)
$/gallery$
$/code$

### Heatmap `plot()`

heatmap plot
- `plot()` with `scheme=2`

$code$
```r
plot(mod_2d, scheme=2)
```

$widec$
$down
$/widec$

$gallery$
![Image](img$4axh)
$/gallery$

- simplified contour map on top of which colours are added
- yellow = larger predictions
- red = smaller predictions
	- nvda. this doesn't make *any* sense!!!
$/code$

### Custom interaction plots with `vis.gam`

$code$
```r
vis.gam(x,
        view = NULL,
        cond = list(),
        n.grid = 30,
        too.far = 0,
        col = NA,
        color = "heat",
        contour.col = NULL,
        se = -1,
        type = "link",
        plot.type = "persp",
        zlim = NULL,
        nCol = 50,
        ...)
```
$/code$

#### Variables and kind of plot

$code$
```r
vis.gam(x = mod,                # GAM object
        view = c("x1", "x2"),   # variables
        plot.type = "persp")    # kind of plot 
```

1. GAM object
2. variables you want to see in the plot
3. the kind of plot ($see $down)

$gallery$
|`persp`|`contour`|
|---|---|
![Image](img$hwlf)|![Image](img$cxge)|
$/gallery$

$info$
In this case, x1 and x2 are interacting variables, but they do not need to be. You can view a perspective plot of any two variables in the model to see their combined effects.
$/info$
$/code$

#### Options for contour plots

##### Hedging predictions

`too.far`
- indicates what predictions should not be plotted because they are too far from the actual data
	- how far is too far to extrapolate?
- scaled from 0 to 1 (along the range of the variables)

$code$
```r
vis.gam(mod, view = c("x1", "x2"),
	    plot.type = "contour", too.far = 0.1)
vis.gam(mod, view = c("x1", "x2"),
		plot.type = "contour", too.far = 0.05)
```

$gallery$
![Image](img$1o0i)
$/gallery$
$/code$

##### Colour and contrast

`color`
- selects the background color palette

`contour.col`
- selects the color of lines

`nlevels
- lets you control the number of contour lines
- important for showing details and subtleties of interactions

$code$
```r
vis.gam(g, view = c("x1", "x2"), plot.type = "contour",
		color = "gray")
vis.gam(g, view = c("x1", "x2"), plot.type = "contour",
		contour.col = "blue")
vis.gam(g, view = c("x1", "x2"), plot.type = "contour",
		nlevels = 20)
```

$gallery$
![Image](img$j1jk)
$/gallery$
$/code$

#### Options for perspective plots

##### Confidence intervals

`"se"`
- number of standard errors away from the average prediction to plot high- and low-prediction surfaces

$code$
**Confidence intervals**

```r
vis.gam(x = mod, view = c("x1", "x2"), 
        plot.type = "persp", se = 2)
```

$gallery$
![Image](img$75hn)
$/gallery$
$/code$

##### Rotation

`theta`
- horizontal rotation

`phi`
- vertical rotation

`r`
- how zoomed is the plot?

$code$
```r
vis.gam(g, view = c("x1", "x2"), plot.type = "persp",
		theta = 220)
vis.gam(g, view = c("x1", "x2"), plot.type = "persp",
		phi = 55)
vis.gam(g, view = c("x1", "x2"), plot.type = "persp",
		r = 0.1)
```

$gallery$
![Image](img$zogr)
$/gallery$
$/code$

$p=8$

## Visualizing categorical-continuous interactions

### Recap: factor-smooth interactions

factor-smooth interactions
- models where we used the `"by"` argument in a smooth to fit different smooths for each value of a categorical variable

$code$
```r
model4b <- gam(hw.mpg ~ s(weight, by = fuel) + fuel,
			   data = mpg, method = "REML")
```

$gallery$
![Image](img$tx87)
$/gallery$
$/code$

### Factor-smooths

factor-smooth
- uses a factor-smooth basis type (`bs="fs"`)

$code$
```r
model4c <- gam(hw.mpg ~ s(weight, fuel, bs = "fs"),
               data = mpg,
               method = "REML")
```

```r
summary(model4c)
```

$widec$
$down
$/widec$

```r
Family: gaussian 
Link function: identity 
 
Formula:
hw.mpg ~ s(weight, fuel, bs = "fs")
 
Parametric coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   28.644      7.615   3.761 0.000223 ***
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
Approximate significance of smooth terms:
                edf Ref.df     F p-value    
s(weight,fuel) 7.71     19 53.12  <2e-16 ***
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
R-sq.(adj) =  0.832   Deviance explained = 83.8%
-REML = 518.54  Scale est. = 7.9735    n = 205
```

$wide$
- no different term for each level of the categorical variable -> one ==overall interaction term==
- => not as good for distinguishing between categories
$/wide$

$wide$
- but: smooths are good for controlling the effects of categories that are *not* our main variable of interest
- especially when there are very _many_ categories, or only a few data points in some categories
$/wide$
$/code$

$info$
In this case, we do not include an additional linear term to make separate intercepts for each level. The factor-smooth formulation accounts for this automatically.
$/info$

### Plotting factor-smooths

$code$
```r
plot(model4c)
vis.gam(model4c, theta = 125, plot.type = "persp")
```

$gallery$
![Image](img$nk6y)
$/gallery$

- `plot()`: will, by default, make one plot with multiple smooths on it
- `vis.gam()`: staircase-like perspective plots 
	- helpful for comparing the shapes of different smooths
$/code$

$p=11$

## Interactions with different scales

tensor smooths
- let us model ==interactions that operate on different scales==, such as space and time

### The problem with a single lambda parameter

$acco$
2D smooths of the form `s(x1, x2)`
- have ==smoothing parameter==, or lambda value, that ==defines wiggliness==
- single lambda value for the whole 2D smooth
$/acco$

$widec$
$contrast
$/widec$

$acco$
complex situations
- having a single smoothing parameter does not make sense

$code$
```r
        x      y  elev    om
 1 181072 333611  7.91   13.6
 2 181025 333558  6.98   14  
 3 181165 333537  7.8    13  
 4 181298 333484  7.66   8  
 5 181307 333330  7.48   8.7
 6 181390 333260  7.79   7.8
 7 181165 333370  8.22   9.2
 8 181027 333363  8.49   9.5
 9 181060 333231  8.67   10.6
10 181232 333168  9.05   6.3
```

- variables for x and y, as well as elevation (all in meters)
- we would expect the ==scale of change== to be similar horizontally along x and y, but it ==could be much different on a per-meter basis with elevation, where small differences could mean very different environments==
	- terms would likely not have the same wiggliness
- similarly, it would not make sense to use the same wiggliness to model distance and the `om` variable, which is organic matter measured in grams per kilogram -> they have incomparable units of measure.
$/code$
$/acco$

### Tensor smooths

tensor smooths
- more appropriate for interactions of variables with different scales or units
- similar to a regular two-dimensional smooth, but it ==has two smoothing parameters== -> one for each variable

$eq$
$bigtex$
$$
y = te(x_1, x2)
$$
$/bigtex$

$widec$
with smoothing parameters $\lambda_1$, $\lambda_2$
$/widec$
$/eq$

`te()`
- code for a tensor smooth in R

$code$
```r
gam(y ~ te(x1, x2), data = data, method = "REML")
gam(y ~ te(x1, x2, k = c(10, 20)), data = data, method = "REML")
```

$info$
Since there are multiple smoothing parameters, you can specify a different number of basis functions, or k values, for each smooth.
$/info$
$/code$

### Example: Fitting with tensors

$gallery$
![Image](img$qykx)

- **left:** shows actual relationship between two variables (x1, x2) + outcome
	 - x1 and x2 operate on different scales
- **middle:** uses `s()` -> assumes both variables vary similarly
- **right:** uses `te()` -> both variables can have different smoothing parameters
$/gallery$

### Tensor interactions

tensor smooths interaction behaviour
- can be used to separate out interactions from individual univariate effects
- model ==only the interaction of two variables==, and ==not their independent effects==
	- are estimated separately!

$eq$
$bigtex$
$$
y = s(x_1) + s(x_2) + ti(x_1, x2)
$$
$/bigtex$

$widec$
with smoothing parameters $\lambda_1$, $\lambda_2$, $\lambda_3$, $\lambda_4$
$/widec$
$/eq$

$code$
```r
gam(y ~ s(x1) + s(x2) + ti(x1, x2), data = data, method = "REML")
```

- regular smooth terms (`s()`) used to model each univariate smooth
- tensor interactions (`ti()`) used to model tensor interactions

$info$
Note that each of these components has its own smoothing parameters and can have its own number of basis functions. This means we are estimating more parameters. Necessarily, such models need more data.
$/info$

$widec$
$down
$/widec$

```r
Family: gaussian 
Link function: identity 
 
Formula:
y ~ s(x1) + s(x2) + ti(x1, x2)
 
Parametric coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 0.318698   0.008697   36.65   <2e-16 ***
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
Approximate significance of smooth terms:
            edf Ref.df     F  p-value    
te(x1)      4.93  6.009 23.16  < 2e-16 ***    # Separate terms for 
te(x2)      3.42  4.242 10.35 2.75e-08 ***    # each variable and
ti(x1,x2) 10.15 12.763 16.08  < 2e-16 ***     # the interaction
 ---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
 
R-sq.(adj) =  0.444   Deviance explained = 46.5%
-REML = -85.566  Scale est. = 0.037067  n = 500
```

- separate smooth terms in the model for each variable, as well as the interaction
- allows us to evaluate the significance of these components independently
$/code$

$code$
```r
gam(y ~ s(x1) + s(x2) + ti(x1, x2), data = data, method = "REML")
```

$gallery$
**Plots of the model**

![Image](img$cggc)
$/gallery$

- univariate smooths for x1 and x2
- surface representing the interaction

$info$
All layers of the model remain **additive**! The univariate smooths are additive, and then the interaction is an addition effect on top of those. Separating the effects in this way makes complex models more understandable.
$/info$
$/code$