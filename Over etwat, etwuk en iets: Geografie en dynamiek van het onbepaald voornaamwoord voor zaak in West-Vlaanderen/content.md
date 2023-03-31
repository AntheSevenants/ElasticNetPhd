$public=true$

# Over etwat, etwuk en iets: Geografie en dynamiek van het onbepaald voornaamwoord voor zaak in West-Vlaanderen

$p=31$

$reader wide$
**Abstract**

This paper focuses on the geography and dynamics of indefinite pronoun
variants in West-Flanders (Belgium). Whereas traditional dialect data show
etwat (‘something’) as being the traditional West-Flemish dialect variant, recent studies have attested a new variant in the area: etwuk. This paper addresses the questions (1) where this new dialect variant is used, (2) by whom,
(3) how it relates to other variants of the indefinite pronoun and to the interrogative pronoun variants and (4) whether the grammatical context in which
it occurs is of any influence. To answer these questions, we analyse 10.000
surveys collected in 2018 in 249 West-Flemish locations. Given that a sample
of this size is difficult to analyse using traditional dialectological methods, we
introduce generalized additive mixed-effects regression as a means of simultaneously analyzing the diatopic, diastratic and diachronic variation in these
surveys (cf. Wieling et al. 2014:689). These generalized additive mixed-effects
models reveal striking dynamics in the pronominal system in West-Flanders:
the variant etwuk is clearly taking over the role of etwat as dialectal indefinite pronoun, with the region of the cities Ieper and Poperinge as ‘expansion
tank’. The rise of the non-standard pronoun etwuk is remarkable, given that
Flanders is generally marked by dialect shift and dialect levelling (usually in
favour of the standard language). We will argue that the pronominal changes
in West-Flanders can be interpreted as a sign of linguistic glocalisation, with
etwuk as means of indexing regional identity in times of homogenizing informal language use
$/reader wide$

$p=32$

## Inleiding

*wuk*
- oorspronkelijk enkel te vinden in de streek Ieper-Poperinge
- maar: nu (2018) te vinden in heel West-Vlaanderen

methode
- _generalized additive mixed-effects regressie_ (Wood 2017)

$p=33$

## Status quaestionis

vorming van onbepaald voornaamwoord
- *et* + vragend woord

$p=34$

*etwuk*
- nieuw onbepaald voornaamwoord
- ontstaan uit *et* + *wuk* (naar analogie met *etwat*)

$p=35$

## Een sociografische studie

### Dataverzameling

$p=36$

datapunten
- 10.000 (!!!)

$gallery$
![Image](img$wm5m)

- vooral jonge hoogopgeleiden uit het zuidoosten van de provincie
- overal meer vrouwen dan mannen
$/gallery$

### Generalized additive mixed modeling

#### Over het model

doel
- kaart maken (dialectologen <3 kaarten)
- sociale variatie meten

$p=37$

$gallery$
**Gebruikte streekonderscheidingen**

![Image](img$a5at)
$/gallery$

$p=38$

$result methodologisch probleem
- niet evident om simultaan *spatiale* en *sociale* patronen te analyseren en visualiseren

$widec$
$down oplossing

_generalized additive mixed modelling_
$/widec$

~= lineaire regressie
- relatie vatten tussen voorkomen van een variant en onafhankelijke variabelen

!= lineaire regressie
- lineaire regressie gaat uit van ==lineaire relatie== tussen predictoren en afhankelijke variabele
- GAM: relatie hoeft niet lineair te zijn -> regressielijn kan ==kronkelen==

$p=39$

uitdaging
- graad van ==niet-lineariteit== voldoende hoog houden -> betrouwbare voorspellingen doen
- maar: niet *te* veel golving -> ==overfitting vermijden==

$info$
De mogelijkheid om ook niet-lineaire relaties te vatten bij GAM
is voor onze studie zeer belangrijk: als we de relatie tussen bijvoorbeeld het
gebruik van _etwat_ en de geografische locatie (geoperationaliseerd in termen van lengtegraad en breedtegraad) willen analyseren en visualiseren,
dan moeten we immers ook niet-lineaire relaties kunnen vatten. Het valt
immers te verwachten dat het gebruik van of de kans op _etwat_ niet strikt
lineair zal toenemen of afnemen met toenames of afnames in breedte- of
lengtegraad; ‘kronkels’ of golven kunnen voorkomen. 
$/info$

softwarepakket
- `mgcv`

#### Operationalisering

type model
- logistisch regressiemodel
- de afhankelijke variabele binair wordt opgevat
- => bijvoorbeeld: 1 = gebruik van _etwuk_, 0 = andere variant

wat modelleren
- kans 𝑝 op bepaalde uitkomst 𝑦
- berekend op basis van gegeven predictoren
- afhankelijke variabele getransformeerd met *logit*-functie

*mixed effects*
- ook gebruikt in dit onderzoek
- variabelen waarvan de levels willekeurige steekproeven zijn van een veel grotere populatie
- random effect: *Kloeke*-locatie

$info$
Het grote voordeel van
deze mixed effect-aanpak is dat mogelijke spreker- of locatiegebonden idiosyncrasieën in rekening worden gebracht.
$/info$

$p=40$

#### Keuzes van de predictoren

moeilijke vraag bij regressie
- $see $down

1. ==hoeveel predictoren==
2. ==welke predictoren==
3. ==welke _interacties_ tussen predictoren== in model opnemen"

te weinig predictoren
- risico op voorbijgaan aan de complexiteit van de variabiliteit van de afhankelijke variableen

te veel predcitoren
- gevaar van *overfitting*

$widec$
$down

deze studie = complex model
$/widec$

1. alle sociale variabelen (diploma, geslacht en geboortejaar)
2. de zin waarin het onbepaald voornaamwoord opgevraagd werd als linguïstische predictor
3. *tensor product based smooth*: zowel lengte- en breedtegraad als geboortejaar opgenomen
	- denkbaar dat het regionale patroon verschilt naarmate respondenten ouder/jonger worden
4. random effect voor Kloeke-code
	- sommige locaties uitzonderingen kunnen uitzonderingen vormen op ‘vloeiende curven’ in termen van lengte- en breedtegraad
	- ook als proxy voor respondentgebonden variatie

#### Incrementeel model

**1.** stapsgewijs modellen genereren
- eerst zonder random effect, dan met simpel geboortejaar, etc.
- telkens incrementeel vergelijken met _Akaike Information Criterion_ (cf. Wieling 2018:12)
	- vergelijkt *goodness of fit* van de twee modellen (met rekening van complexiteit van de modellen)
- ook met `compareML`-functie van het R-pakket `itsadug`, dat ook de significantie test van eventuele verschillen in
ML-scores


**2.** `gam.check() in het R-pakket `mgcv`
- hebben we de niet-lineaire patronen in het uiteindelijke model niet te zeer vereenvoudigd?
- ~ Wieling (2018:9): we stellen het aantal mogelijke basisdimensies hoger in als de verkregen _k_-index kleiner is dan 1 en de bijhorende p-waarde laag is

$p=41$

## Resultaten

### Frequentie en regionale verspreiding van de varianten

[ Absolute frequenties van voornaamwoorden ]
|type|frequentie|
|---|---|
|_etwat_|19637|
|_etwuk_|11587|
|_iets_|3684|
|_iet_|1245|
|_wat_|1055|
|_wuk_|23|
|_ieks_|17|
|_etwiene_|11|

$p=42$

regionale patronen
- *etwat, *etwuk*, *iets* en *iet* komen voor in zowat heel West-Vlaanderen

$info$
De ruime verspreiding van _etwat_, _etwuk_, _iets_ en _iet_ betekent niet dat de
varianten overal in West-Vlaanderen even sterk staan. In heel wat meetpunten, waar verschillende (tientallen) respondenten de enquête hebben ingevuld, bestaat er wel degelijk een uitgesproken voorkeur voor deze
of gene vorm. 
$/info$

$gallery port$
**Voorspelling van de probabiliteit van de frequentste varianten**

$widec$
![Image](img$sxsl)
$/widec$

$widec$
(figuur p. 44)
$/widec$

- de vorm ==_etwat_== staat volgens het GAM-model vooral in het ==noorden van
West-Vlaanderen== sterk
- de vorm ==_etwuk_== komt frequenter voor in de streek van ==Ieper-Poperinge==
- de varianten ==_iet_ en _iets_== lijken conform het oudere SAND-materiaal
vooral in ==zuidoosten van de provincie== (de streek van Waregem) sterk te
staan
- de hoogste probabiliteiten voor de vorm ==_wat_== vinden we in het ==uiterste
oosten van de provincie==
$/gallery port$

$p=43$

### Impact van leeftijd, geslacht en opleidingsgraad

#### Leeftijd

leeftijd
- belangrijkste predictor

$gallery$
$widec$
![Image](img$t2w2)

(figuur p. 45)
$/widec$

- ==_etwuk_ frequenter naarmate de informanten jonger worden==
	- ten koste van de traditionele variant _etwat_
	- 1983 blijkt een kantelpunt
- _iets_, _wat_ en _iet_ -> minder leeftijdsgevoelig
$/gallery$

$result conclusie
- _apparent time_: opmars van *etwuk*, regressie van *etwat*

$warn$
**Is er dan geen age grading?**

Bij age grading verwachten we immers een ==u- of v-achtige curve==, waarbij een variant piekt/afneemt op een specifiek moment (doorgaans tijdens de adolescentie), en daarna weer afneemt/toeneemt. In Figuur 5 daarentegen zien we een ==gestage toename van de variant== _etwuk_, waardoor een apparent-time interpretatie (in termen van taalverandering op gemeenschapsniveau) meer voor de hand
ligt.
$/warn$

regionaal leeftijdseffect
- *etwuk* het sterkste in de regio Ieper-Poperinge
- voortrekkersrol bij opmars van *etwuk*

$gallery port$
**Leeftijd per regio**

$widec$
![Image](img$fhig)
$/widec$
$/gallery port$

$p=45$

#### Geslacht

$wide$
niets noemenswaardig
$/wide$

#### Opleidingsgraad

$wide$
moeilijk te interpreteren door correlatie met leeftijd
$/wide$

$p=48$

### Impact van grammaticale context

verschillende contexten in de enquête
- $see $down

1. ongemarkeerd  
	_Ga je ook iets eten nu?_
2. benadrukt  
	_Ja maar, je moet toch íets eten!_
3. in combinatie met bijvoeglijk naamwoord  
	_Ik heb daar in de winkel iets schoons gekocht._
4. voorafgegaan door voorzetsel  
	_Heb je dat voor iets nodig?_

$p=49$

impact van context
- bestaat en is significant!
- maar wel beperkt

$gallery$
![Image](img$fcro)
$/gallery$

$p=50$

## Discussie en besluit

expansie van *etwuk*
- zowel geografisch als diachroon
- er is sprake van *dialectverandering*
- in de toekomst: *etwuk* stoot *etwat* (traditioneel) waarschijnlijk van de troon

$p=51$

waarom *wuk* en *etwuk* populair?
- *glocalisering*
- dialectwoordenschat gaat verloren, maar in meer algemene informele standaardtaal moet regionale identiteit toch doorklinken
- bv. met regionale elementen zoals *etwuk* en *wuk*
	- duidelijk anders dan de standaardtalige voornaamwoorden

$info$
Het bestaan
van een West-Vlaamse regionale identiteit blijkt onder andere uit het gegeven dat onze enquête, verspreid met als onderwerp ‘West-Vlamingen
gezocht’, in slechts twee weken tijd 10.000 reacties oogstte en ook grotendeels ongevraagd opgepikt werd door allerlei media (sociale media, maar ook kranten en zelfs radioprogramma’s).
$/info$