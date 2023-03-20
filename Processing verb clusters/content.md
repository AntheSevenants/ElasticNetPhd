$public=true$

# Processing verb clusters

## Introduction

### Verb clusters

$p=4$

[ Types of verb clusters ]
|main clauses|subordinate clauses|
|---|---|
|need at least ==three verbs==|need at least ==two verbs==|

stress influence on choice
- "if there is a stressed syllable directly before the cluster, speakers may choose the R order"
- stress on main verb (participe) = last item
- avoids repeating adjacent stress patterns (De Sutter 2005)

semantic influence on choice
- auxiliary verbs (De Sutter 2005)
- Aktionsart (Pardoens 1991)

$p=5$

$result *many* influences found
- no one magic solution influence
- => necessary **to cover as much ground as possible**

### New methods

$p=6$

automatically annotated corpora
- \+ very large
- \- lower accuracy
- \+ large volumes of data make up for lower accuracy

## Applying automatically parsed corpora to the study of language variation

### Introduction

### Verbal cluster variation

$p=14$

[ Types of verbal clusters ]
|auxiliary cluster|modal cluster|
|---|---|
|*hebben*, *zijn*, *worden*|*willen*, *kunnen* ...|

$p=16$

main clause cluster
- need 3 verbs or more

$example$
- Het kan gedaan worden (G)
- Het kan worden gedaan (R)
$/example$

$info$
Stroop (2009) says the order variation in main/subclauses are influenced by the same factors. So things you say about subclauses should also be true for main clauses.
$/info$

$p=17$

### All influencing factors

**1.** context:  
region and mode of communication
- R order in areas where dialect close to SD
- G order preferred in spoken/interactive communication
- G order less likely in edited writing

**2.** rhythmic factor:  
adjacent stressed are avoided
- main verb carries stress, so R order preferred in case of clash

**3.** semantics:  
particularly ==*lexical* semantics==
- adjectiveness is a common factor
- but: not much work has been done (in general)
- Pardoen (1991): G = stative, R = dynamic

**4.** discourse:  
syntactic persistence / priming
- repeated usage

$p=18$

processing hypothesis  
(Gries 2001)
- "in linguistic ctxs that are more **difficult to to process**, speakers will choose the ctx that requires the **least processing effort**"

$p=21-22$

### Method and data

corpus used = Lassy Large
- but: *only* the Wikipedia part! (= neutral)
- xpath + DACT

$p=24$

### Results

results found
- ~= De Sutter 2005 (so he's done a good job)

$p=29$

### Discussion

"default word order"?
- Meyer & Weerman (2017): "R is the default"
- but: evidence is inconclusive

$p=30-31$

$reader wide$
Multilevel modeling may be used as in Bouma (2017b) to model the effects of individual lexical items. A form of Principal Component
Analysis could be applied to generalize over the variables and try to reduce
their number.
$/reader wide$

$p=47$

## Verbal cluster order and processing complexity

$p=59$

### The processing hypothesis

processing hypothesis
- De Sutter (2005): G is the default order
	- \+ Zuckerman (2001): young children use the G order more frequently
- Meyer & Weerman (2016): R is the default order

$p=61$

R order as proxy for language contact?
- more language contact = more R?

$p=63$

### Method and data

Lassy Large, Wikipedia
- chosen to represent 'average' Standard Dutch

$p=89$

### Conclusion

conclusion
- R order thought to be the "default" -> used the most in difficult situations

$p=107$

## Lexical effects on Dutch verbal cluster order

### Introduction

lexical effects
- associated between **verbs** and **verb clusters**
- semantic explanations for choice between both

$p=110$

frequency
- entrenches linguistic elements in memory
- i.e. speakers grow accustomed to a verb having a specific order in a verb cluster, then repeat that order
- => ($todo check correlation coefficient and frequency!)

$p=111$

### Previous work

previous work
- Pardoen (1991): R order is associated with **==dynamic== interpretation** of verbs
- De Sutter (2005): tentative agree
	- problem some stative verbs are in the R order (e.g. *zijn*)
	- G order also has stronger associations
	- relatively few words have a significant association
		- either sample size too small/associations do not exist

$info$
1.7 million words was found to be **too small** for lexical associations. Sparsity issues are why "not often investigated".
$/info$

$p=112-113$

### Lexical and semantic associations

corpus
- Lassy Large, Wikipedia

lexical associations
- associated with ==many== different factor
	- can be a consequence of more *general* factors (e.g. complexity, prosody ...)
	- controlling for them -> **important**

lexical association / lexical idiomaticity
- (Hindle & Rooth, 1993), (Speelman et al. 2009)
- "when an associated cannot be accounted for with other factors, it must be lexical"

$p=114$

explanations for lexical preferences
- will not be given
- each verb is different, could be for historical reasons

$p=115$

possible hypotheses
- Pardoen (1991): stative <-> dynamic
- De Sutter (2005): adjectiveness ~ G

### Measuring lexical associations

#### Collostructional analysis

==collexeme==
- a lexeme strongly associated with a specific construction

==collostruction==
- the construction

$warn$
The problem with collostructional analysis is that it **does not control for other factors**! Only **frequency** is taken into account.
$/warn$

$p=116$

Stefanovitsch & Gries (2003)
- Fischer's exact test
- can handle low-frequency items
- no distributional assumptions
- input = ${2}\times2$ contingency table

| |ctx|$negctx|
|---|---|---|
|verb|...|...|
|$negverb|...|...|

$result output
- p value which measures **association**

analysis
- top 𝑛 items are classified according to semantic and syntactic properties

$info$
The technique was later adapted into *distinctive* collexeme analysis for comparing competing ctx.
$/info$

$p=117$

#### Gain Ratio

gain ratio
- attempt to quantify the effect of lexical associations <-> other factors
- 'measure of informativity'
	- quantifies the informativity of each factor
	- here: 'gain ratio'
	- based on 'information gain' from model *with* and *without* main verb facor
- alternative for model with random effects
	- of course, such models would fail to converge with too many levels

$reader wide$
"Generalization to a linear term [...] is an impossible task when there are many unique verbs -- you can't create a line from a single data point."
$/reader wide$

$p=118$

#### Residuals of logistic regression

residuals
- for each data point in a maximal regression model, fetch its residuals
- then, average residuals per main verb
- resulting residual value = remaining variationo
	- either due to lexical contribution of the verb, or some other factor

### Verb clusters

$p=119$

clusters with modal verbs  
(e.g. *kunnen*)
- used in R order most of the time
- Bloem considers them 'different' to the other clusters

$p=121$

### Method and data

overview of factors and lexical influence
- indirect link: middle field length, clause type
- direct link: morphological complexity

$p=122$

frequency cut-off
- most incorrect parses in the corpus are for low-frequency items
- (also: manual samples and correction)

$p=123$

$reader wide$
"The size of the dataset is very important in collostructional analysis"
$/reader wide$

$p=125-126$

### Results

results
- two verbs occur exclusively in either order
	- *toebehoren* + *malen*
- computed association = more importnat than signifiance (large size makes anything significant *anyways*)
	- => ==good for Elastic Net, because we don't have significance==

$p=127$

correlation with frequency?
- likely!
- threshold = OR 1.5 (or 0.66) -- $todo dit herbekijken

$reader wide$
"these word order associations are a NOT marginal phenomenon that occurs with uncommon verbs"

- $result BUT! they are ==more common== across lower-frequency verbs
$/reader wide$

$p=128$

ways of operationalising adjectiveness
- $see down
	1. zijn-ratio
		- percentage of clusters with *zijn*
	2. ratio of adjective use
		- problem: tagging is unreliable!
		- if I read it correctly, Bloem only uses undeclensed forms

$p=129$

disturbing factors
- here: separability
- gleaned from plotting distribution of proportions
- $todo zelf kijken hiernaar

$p=130$

auxiliary verbs have distinct word order associations
- Bloem et al. (2014): *hebben* = more R
- if a verb predominantly occurs with *hebben*, the results would be skewed
- solution: only look at *hebben*
	- also evades adjectiveness issues

$p=132$

semantic analysis
- done using ==Cornetto==!

$p=133$

Pardoen (1991)
- could not be proven
- => no stative/dynamic differences

$p=134$

hints
- negative polarity? -> more G
- semantically similar verbs have similar associations -> cluster order

$p=137$

verb cluster associations
- "verb cluster word associations are also the result of lexical preference for one of the word orders"

most prevalent associations
- negative verbs
- cognition verbs

$p=138$

### Conclusions and future work

$reader wide$
"no other major categories of verbs in the data with different order preferences"

- $result means: probably no other influences -- $todo herbekijken
$/reader wide$

stronger associations with G order (~= De Sutter 2005)
- Bloem thinks this is a **frequency effect**
- R is more frequent -> threshold for specificity is simply higher

$p=139$

$reader$
"perhaps more semantic associations of verb clusters can be found if different or additional semantic categories of verbs are used [...] or based on ==**semantic similarity**== using ==**distributional semantics**=="
$/reader$

## Synchronic variation and diachronic change in Dutch two-verb clusters

### Introduction

$p=194$

variation btwn two orders has not been stable over time
- 1400: almost exclusively G
- since then: rise of R order
- Coussé (2008): "this is still happening"
- => here: ==apparent-time== study

$p=197$

### Background: variation and change in the order of Dutch verb clusters

overview of relative popularity (Coussé 2008)
- 1250 / RG
- 1400 / G dominant
- 1500-1600 / $up R
	- first through modal auxiliaries!

the role of acquisition
- Meyer & Weerman (2016): R order is the default in acquisition -> learnt first
- "children must learn how to deviate from the default, and may do so imperfectly"
- with R as the default, it becomes a =='catch-all'==

$p=199$

### Relation between diachronic change and synchronic variation

synchronic variation as a result of diachronic change
- "younger speakers may represent a newer stage"
- speakers do not substantially change their grammars after the critical period -> relatively stable (Boberg 2004)

$p=200$

==apparent-time method==  
(Labov 1965)
- synchronic variation btwn agre groups may be due to **ongoing language change**

/!\ ==age-grading==
- a stable situation with variation across ages
- can cloud the picture of ongoing change

$info$
**How to know for sure?**

Look at real longitudinal data -> is change already ongoing? If so, you're probably looking at ongoing change.
$/info$

$warn$
**'Language = stable?'**

*Not* for lexical content! (Meyerhoff 2006) -> you should focus on $todo
$/warn$

$p=201$

hypothesis
- younger speakers use R order more often
	(bcs change is already in progress)

$p=202$

### Method and analysis

method
- CGN corpus

$p=206$

check distribution btwn regions using $\Chi^2$
- different, so only focus on NL
- also: limited to only spontaneous conversations

$p=210$

### Results

results
- G order is still very frequent in spontaneous speech
- younger group uses R the most, and there is an increasing trend

$p=212$

results for each auxiliary separately
- *hebben* shows the most R (as expected)

$p=215$

### Discussion and conclusion

younger group behaviour
- very sudden increase
- due to age grading? or acquisition that is not yet ocmpleted?
- G is very prevalent in dialects -> young speakers generally no longer speak dialect

$p=216$

why are we moving towards R order?
- regional factors? processing complexity?
- language context tends to lead to more G orders
- => maybe younger speakers have come more into contact with, i.e., English (hence a higher percentage of R, bcs English is a red-only language)
- => we could also just be in the acceleration part of the S-curve