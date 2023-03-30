$public=true$

# A radically data-driven Construction Grammar: Experiments with Dutch causative constructions

$p=17$

constructions
- pairings of form and function (Goldberg)

$widec$
$down
$/widec$

slot fillers
- a convenient heuristic to access conventional uses of a construction
- semantic classes:
	1. can indicate different senses
	2. may reflect division of semantic labour between constructions

$p=18$

problem with 'ad hoc' classifications
- mostly based on *intuition* => arbitrariness
- solution = frameworks like WordNet, but still arbitrary
	- also comes with its own set of problems

$p=19$

this paper
- 'optimal' classification using distributional semantics
- using alternation: NLD *doen* <-> *laten*

$p=21$

*doen* <-> *laten*
- *laten*: human causers
- *doen*: non-human causers
- high effect of semantic classes: *Causer* and *Effected*

$p=25$

data source
- similarity matrix (e.g. cosine)

$p=26$

effect of window (Peirsman, Heylen & Geeraerts 2008)
- larger context window: looser, more associative relations
	- e.g. _doctor-hospital_, _bird-sky_
- smaller context window: tighter, taxonomic relations
	- e.g. _doctor-nurse_, _bird-robin_
- effect of relation: i.e. dependency / BOW

$p=27$

dependency models  
(Heylen et al. 2008/2009)
- find even tighter semantic relations than small BOW
- especially good at finding near-synonyms  
	(e.g. *hospital-clinic*, *monkey-ape*)

$p=33$

hierarchical cluster analysis  
(Everitt et al. 2001)
- groups noun and verb lemmata into semantic classes
- experiments ranging 5 to 100 classes

$p=34$

evaluation
- power of classes in predicting the use of *doen*/*laten*
- predictive power will be greater if the lexemes that belong to one class tend to be used with only one of the two auxiliaries
- R², Gamma and AIC

$p=35-36$

$gallery port$
**Causer**

$widec$
![Image](img$0s8l)
$/widec$

- C index rapidly goes up from the very beginning, which indicates that the
relevant semantic distinctions are captured by a relatively small number of classes
- syntactically enriched model performs much better than the simple bag-of-words model
	- corroborates the results in Gries & Stefanowitsch 2010
- performance per no of classes
	- 2 classes: already 0.69
	- 6 classes: 0.80

$info$
The 100-cluster syntactically enriched solution has the highest value
(C = 0.89), but its predictive power is not dramatically different from the more
parsimonious classifications with a smaller number of classes.
$/info$
$/gallery port$

$p=36$

----
$widec$
Here: 6 cluster classification
$/widec$
----

$acco$
cluster 1
- predominantly inanimate concrete and abstract nouns
- e.g. _cd_ ‘cd’, _cijfer_ ‘digit’, _herstel_ ‘recovery’, _stem_ ‘voice’, _aanslag_ ‘attack’, _afwezigheid_
‘absence’, _resultaat_ ‘result’
$/acco$

$wide$
- $result majority are instances of the construction *doen*
$/wide$

$acco$
cluster 2
- mostly football- and music-related nouns denoting people and organisations
- e.g. _feyenoord_ (a football club in the netherlands), _dirigent_ ‘conductor’, _speler_ ‘player’,
_orkest_ ‘orchestra’
- some exceptions

cluster 3
- some proper names of conductors and common and proper nouns denoting
political and other agents
- e.g. _gergiev_ (a russian conductor), _van hecke_ (a belgian
politician), _secretaris-generaal_ ‘general secretary’, _harnoncourt_ (an austrian conductor) etc.

cluster 4
- many geographical names, which are frequently
used in newspaper articles to refer to the government metonymically
- e.g. _verenigde staten_ ‘the us’, _amerika_, _washington_, etc.

cluster 5
- mostly common nouns, which denote people in charge and organisations
- e.g. _regering_ ‘government’, _minister_ ‘minister’, _bedrijf_ ‘company’, _trainer_ ‘trainer’
$/acco$

$wide$
- $result majority occur more frequently with *laten*
$/wide$

$acco$
cluster 6
- only 7 nouns with very low collocability due to an extremely low frequency
$/acco$

$wide$
- $result too small for evaluation
$/wide$

$p=36-37$

$gallery port$
**Causee**

$widec$
![Image](img$hibj)
$/widec$

- poor predictive power
- category is largely irrelevant
$/gallery port$

$p=37-38$

$gallery port$
**Effected**

$widec$
![Image](img$lg6x)
$/widec$

- when number of clusters is small, different leader
- but as more clusters, other models take over
- bag-of-words models performed on average worse than most other models
$/gallery port$

$p=40-41$

$gallery port$
**Combined classes of three slots**

$widec$
![Image](img$eu68)
$/widec$

- adding the semantic
information about the Causer and Causee to the information about the Effected
Predicate does increase the predictive power, but the effect is ==non-additive==
$/gallery port$

$p=41-42$

advantages
- $see $down

1. find the optimal semantic classification of constructional
slot fillers, based on their ==usage==
2. take into account the ==frequencies of semantic classes== in near-synonymous
constructions
3. ==objective evidence== about the optimal level of semantic granularity
of semantic descriptions
4. detect ==semantic regularities== which might otherwise go ==unnoticed==
5. explore ==larger amounts of data== than it would be possible manually
	- more robust analyses

$p=42$

non-additiveness
- if a word is compatible with the meaning of the construction, there should be ==coherence== among all
slots (Principle of Semantic Coherence, Stefanowitsch and Gries, 2005: 11)
- information stored in one slot interacts with information from the other slots
- nvda. some form of _redundancy_ maybe?

$p=43$

syntactic models = the best
- syntactic ctx features highlight syntactically relevant semantic properties of lexemes