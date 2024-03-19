# Sparse overcomplete word vector representations

$public=true$

$reader$
Current distributed representations of
words show little resemblance to theories of lexical semantics. The former
are dense and uninterpretable, the latter largely based on familiar, discrete
classes (e.g., supersenses) and relations
(e.g., synonymy and hypernymy). We propose methods that transform word vectors into sparse (and optionally binary)
vectors. The resulting representations are
more similar to the interpretable features
typically used in NLP, though they are discovered automatically from raw corpora.
Because the vectors are highly sparse, they
are computationally easy to work with.
Most importantly, we find that they outperform the original vectors on benchmark
tasks.
$/reader$

$p=1491$

## Introduction

word vectors
- distributed representations of words
- can be derived directly from unannotated corpora

problem with vectors
- do not resemble traditional topologies from lexical semantics
- not interpretable

$widec$
$down this paper
$/widec$

sparse coding method
- transforms any distributed representation of words into **sparse vectors**
- these can then be transformed into binary vectors

sparse / "overcomplete" representations
- should be more interpretable
- should be more stable

$p=1492$

## Sparse overcomplete word vectors

**1.** method A
- dense vectors -> sparse, overcomplete vectors

**2.** method B
- dense vectors -> sparse + binary overcomplete vectors

$widec$
$down
$/widec$

$acco$
𝑉
- vocabulary size

𝐿
- vector size
$/acco$

$acco$
**𝑋** ∈ ℝ^𝐿×𝑉^
- ==initialising vector==
- the matrix constructed by stacking 𝑉 non-sparse “input” word vectors of length 𝐿

**𝐴** ∈ ℝ^𝐾×𝑉^
- ==overcomplete vector==
- the matrix containing 𝑉 sparse overcomplete word vectors of length 𝐾
$/acco$

$result "overcomplete"?
- implies that 𝐾 > 𝐿

$p=1493$

$gallery$
![Image](img$zd08)
$/gallery$

$p=1492$

### Sparse coding

goal
- represent each input vector **x**~i~ as a sparse linear combination of basis vectors **a**~i~

$eq$
**Loss function**

$$
\text{arg min}_{D,A} \left|\left| X - DA \right|\right|_2^2 + \lambda \Omega(A) + \tau \left|\left| D \right|\right|_2^2
$$

$widec$
**𝐷** ∈ ℝ^𝐿×𝐾^: the dictionary of basis vectors  
λ: regularisation hyperparameter  
Ω: regulariser (= ℓ~1~ penalty)
$/widec$

$info$
Squared loss for the reconstruction error is used.
$/info$
$/eq$

$eq$
**Loss function (for each word vector)**

$$
\text{arg min}_{D,A} \sum_{i=1}^{V} \left|\left| x_i - D a_i \right|\right|_2^2 + \lambda \left|\left| a_i \right|\right|_1 + \tau \left|\left| D \right|\right|_2^2
$$

$widec$
**𝑚**~i~: the *i*^th^ column vector of matrix M
$/widec$
$/eq$

### Sparse non-negative vectors

non-negativity
- "has often been shown to correspond to interpretability"
- (Lee and Seung, 1999; Cichocki et al., 2009; Murphy et al., 2012; Fyshe et al., 2014; Fyshe et al., 2015)

methodology here
- variation of the ==non-negative sparse coding== method (Hoyer, 2002)
- further constrains the problem of $see $up

$eq$
**Loss function (for each word vector)**

- **D** and **a**~i~ must be non-negative

$$
\underset{D \in \mathbb{R}^{L \times K}{\geq 0}, A \in \mathbb{R}^{K \times V}{\geq 0}}{\text{arg min}} \sum_{i=1}^{V} ||x_i - D a_i ||_2^2 + \lambda ||a_i||_1 + \tau ||D||_2^2
$$
$/eq$

### Optimisation

optimiser
- 'online adaptive gradient descent'

$p=1493$

### Binarising transform

$p=1494$

### Hyperparameter tuning

## Experiments

### Effects of transforming vectors

sparse vectors
- work better in downstream tasks

### Effect of vector length

----
$widec$
𝐾 = 𝑎 &middot; 𝐿 where α ∈ {2, 3, 5, 10, 15, 20}
$/widec$
----

$p=1495$

$gallery port$
$widec$
![Image](img$4hii)
$/widec$

- $result 𝑎 = 10 gives the best result
$/gallery port$

### Alternative transformations

$widec$
(do not yield better results)
$/widec$

## Interpretability

### Word intrusion

word intrusion experiments
- seek to quantify the extent to which dimensions of a learned word representation are coherent to humans

$reader$
In one instance of the experiment, a human judge is presented with five words in random order and asked
to select the “intruder.” The words are selected by
the experimenter by choosing one dimension j of
the learned representation, then ranking the words
on that dimension alone. The dimensions are chosen in decreasing order of the variance of their
values across the vocabulary. Four of the words
are the top-ranked words according to _j_, and the
“true” intruder is a word from the bottom half of
the list, chosen to be a word that appears in the top
10% of some other dimension.

An example of an
instance is:
_naval_, _industrial_, _technological_, _marine_, _identity_
$/reader$

$p=1496$

$reader$
Results in Table 5 confirm that the sparse overcomplete vectors are more interpretable than the
dense vectors.
$/reader$

### Qualitative evaluation of interpretability

<table>
<caption>Top-ranked words per dimension for initial and sparsified GC representations. Each line
shows words from a different dimension.</caption>
<tr>
<th>
X
</th>
<td>
combat, guard, honor, bow, trim, naval<br>
’ll, could, faced, lacking, seriously, scored<br>
see, n’t, recommended, depending, part<br>
due, positive, equal, focus, respect, better<br>
sergeant, comments, critics, she, videos
</td>
</tr>
<tr>
<th>
A
</th>
<td>
fracture, breathing, wound, tissue, relief<br>
relationships, connections, identity, relations<br>
files, bills, titles, collections, poems, songs<br>
naval, industrial, technological, marine<br>
stadium, belt, championship, toll, ride, coach
</td>
</tr>
</table>