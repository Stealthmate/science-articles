---
author: Valeri
title: Translating Weisfaler and Leman's graph isomorphism paper into layman's English
layout: post
toc: true
last_edit: 2022-02-20
abstract: Weisfeiler and Leman wrote a famous paper called The reduction of a graph to canonical form and the algebra which appears therein, which gives rise to an algorithm for testing if two graphs are isomorphic. Problem is, this paper is <it>really</it> hard to understand, so I decided to write an article explaining everything in detail.
---

## The basic idea

asdasd

## The construction

Consider a graph $\Gamma$ on $n$ vertices. Let $A(\Gamma)$ be its adjacency matrix, that is the matrix whose entry $a_{i,j}$ is equal to the number of edges from vertex $v_i$ to vertex $v_j$. We are going to construct a matrix $X(\Gamma)$ whose entries $x_{i,j}$ are _independent variables_. Every entry $x_{i,i}$ on the main diagonal contains an independent variable $\gamma_k$ and every other entry contains an independent variable $\chi_k$. We use the notation $x_{i,j} = \chi_k$ or $x_{i,i} = \gamma_k$ to denote that a certain entry in $X$ is populated by the corresponding variable; we also use $x_{i,j} = x_{i^\prime, j^\prime}$ to mean that the two entries in $X$ are populated by the same variable. With this in mind, we impose the following requirement:

$$
x_{i,j} = x_{i^\prime, j^\prime} = \begin{cases}
\gamma_k & i = j,\\
\chi_k & i \neq j
\end{cases} \iff a_{i,j} = a_{i^\prime, j^\prime}.
$$

That is to say, entries of $X$ which correspond to equal values of $A(\Gamma)$ are populated with the same variable.

Next we define a partial order on all the $\gamma_k$ and $\chi_k$ as follows. First, let $\gamma_i > \chi_j$ for all $i$, $j$, that is to say, variables at the diagonal are always larger than any non-diagonal variable. Addtionally, for all $i$ and $j$, let

$$
x_{i,j} > x_{i^\prime, j^\prime} \iff a_{i, j} > a_{i^\prime, j^\prime},
$$

that is the order between pairs of $\gamma_k$ or $\chi_k$ is the same as the order on the corresponding entries in $A(\Gamma)$. **This ordering is in fact total** and we enumerate all the variables accordingly. Thus, $\gamma_1$ is the variable that corresponds to the smallest $a_{i,i}$ and $\chi_1$ is the variable that corresponds to the smallest $a_{i,j}$ where $i \neq j$.

Let $X^\prime$ be the matrix where each variable $\gamma_k$ or $\chi_k$ in $X$ is replaced by a yet new variable $\gamma^\prime_k$ or $\chi^\prime_k$ accordingly. As previously, we use $x^\prime_{i,j}$ to denote the corresponding entry in $X^\prime$. Using this, we define the following ordering on bilinear terms of $\gamma_k$, $\gamma^\prime_k$, $\chi_k$, $\chi^\prime_k$ where $(i, j)$ denotes an ordered pair of natural numbers.

$$
\chi_i\chi^\prime_j > \chi_k\chi^\prime_l \iff (i, j) > (k, l).
$$

**Note that we use the same definition for terms that contain both $\chi$ and $\gamma$ or $\gamma$ only, that is to say we compare the corresponding indices.**

### Operations on the matrix $X$

Next, we define a group of operations that take $X$ as input and yield a new matrix.

First, let $\alpha_0(X)$ be an operation that relabels the vertices of $\Gamma$ so that in $X^{(1)} = \alpha_0(X)$ we have

$$
i \leq j \iff x^{(1)}_{i, i} = \gamma_k \leq \gamma_l = x^{(1)}_{j, j}.
$$

In other words, $\alpha_0(X)$ permutes the rows and columns of $X$ so that the elements on the diagonal fall in the order described earlier.

Now we define $\alpha_1(X)$ as an operation that replaces the variables in $X$ with new ones. The new variables are also ordered and their enumeration is given by the function $l(i, j)$. Note that we use $l(i) = l(i, i)$ for short. First we define the relationship between values of $l(i, j)$ as follows, with the new notation explained afterwards.

$$
\begin{aligned}
l(i) < l(j) &\iff \Big( f^{I}_i(X), f^{II}_i(X) \Big) < \Big( f^{I}_j(X), f^{II}_j(X) \Big), \\
l(i, j) < l(i^\prime, j^\prime) &\iff F(i, j) < F(i^\prime, j^\prime).
\end{aligned}
$$

$f^{I}_i(X)$ and $f^{II}_i(X)$ denote the $i$-th row and $i$-th column of $X$ respectively, and the tuple $\Big( f^{I}_i(X), f^{II}_i(X) \Big)$ denotes their concatenation. We define $F(i, j)$ as follows.

$$
F(i, j) = \Big( x_{ij,ji}, f^{I}_i(X), f^{II}_i(X), f^{II}_j(X)\Big)
$$

<!-- ## DFA in a nutshell -->

<!-- A DFA (short for *deterministic finite automaton*) is usually stated as a 5-tuple $(Q, \Sigma, \delta, q_0, F)$ where each element is defined as follows: -->

<!-- - $Q$ - the set of all possible states. Keep in mind, a "state" in this context is not a mathematical object. From the automaton's perspective, states need only be different from each other - it doesn't matter what they "are". From a human's perspective though, it is sometimes useful to give each state an interpretation, so that we understand what the automaton is doing in an abstract sense. -->
<!-- - $\Sigma$ - the set of possible inputs to the automaton. Also called the input alphabet. -->
<!-- - $\delta: Q\times\Sigma \to Q$ - the *state transition function*, that is a mapping which, given the current state and an input, evaluates to the next state after consuming the input. -->
<!-- - $q_0 \in Q$ - the initial state of the automaton. -->
<!-- - $F \subseteq Q$ - a subset of the possible states which are considered *final* (or *accepting*). Whether a state is accepting or not depends entirely on the interpretation of the problem. -->

<!-- This is enough to theoretically define an automaton, but we usually introduce another definition for convenience. The *extended transition function* -->

<!-- $$ -->
<!-- \hat\delta : Q \times \Sigma^* \to Q, -->
<!-- $$ -->

<!-- tells us the last state that the automaton enters after reading the whole input. Taking $\epsilon$ as the empty string, $w \in \Sigma^*$ and $a \in \Sigma$ we define $\hat\delta$ as -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta(q, \epsilon) &= q, \\ -->
<!-- \hat\delta(q, wa) &= \delta(\hat\delta(w, q), a), -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- If we call our automaton $M$ and denote the set of strings that are accepted by $M$ as $L(M)$, we can define the acceptance condition for a string $w \in \Sigma^*$ as -->

<!-- $$ -->
<!-- w \in L(M) \overset{def}{\iff} \hat\delta(q_0, w) \in F. -->
<!-- $$ -->

<!-- ## NFA in a nutshell -->

<!-- A *non-deterministic automaton* (NFA for short) is like a DFA, except it can transition to multiple states (or none at all) for a given input. Thus the transition function is defined slithly differently -->

<!-- $$ -->
<!-- \delta: Q \times \Sigma \to \mathcal{P}(Q). -->
<!-- $$ -->

<!-- An top of this, we can also introduce $\epsilon$-transitions, that is, transitions which do not require any input ($\epsilon$ stands for the empty string, as denoted earlier). Such an NFA is given by the 5-tuple $(Q, \Sigma \cup \\{\epsilon\\}, \delta, q_0, F)$. Naturally, we also need to modify the extended transition function as well, but before that we need another concept. -->

<!-- We call -->

<!-- $$ -->
<!-- \Epsilon_1^*: Q \to \mathcal{P}(Q) -->
<!-- $$ -->

<!-- the $\epsilon$ *closure* of $q$ and define it as -->

<!-- $$ -->
<!-- \Epsilon_1^*(q_1) = \{ q \in Q \,|\, \text{There is a path from } q_1 \text{ to } q \text{ that contains only } \epsilon \text{ transitions.}\}. -->
<!-- $$ -->

<!-- We shall use this definition to define the closure for a set of states $S \subseteq Q$ as -->

<!-- $$ -->
<!-- \Epsilon^*(S) = \bigcup_{q \in S} \Epsilon_1^*(q). -->
<!-- $$ -->

<!-- We can now (re-)define the extended transition function: -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta : Q \times\Sigma^* &\to \mathcal{P}(Q) \\ -->
<!-- (q, \epsilon) &\mapsto \Epsilon^*(\{ q \}) \\ -->
<!-- (q, wa) &\mapsto \bigcup_{q_i \in \hat\delta(q, w)} \Epsilon^*(\delta(q_i, a)). -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- The acceptance condition changes as well. A single input string can be resolved to multiple end states, but we define an NFA to accept a string if even a single resolution ends in a final state. Thus, we can state this property (for an NFA called $N$ and a string $w \in \Sigma^*$) as -->

<!-- $$ -->
<!-- w \in L(N) \overset{def}{\iff} \hat\delta(q_0, w) \cap F \neq \emptyset. -->
<!-- $$ -->

<!-- ## The Proof -->

<!-- In order to show that NFA are equivalent, we must show that all DFAs can be represented as an NFA and vice versa. -->

<!-- ### Any DFA can be represented as an NFA -->

<!-- Informally, since an NFA only allows non-deterministic transitions but doesn't require them, any DFA is trivially an NFA. This in itself should be convincing enough, but we still provide a formal proof. -->

<!-- Let $M$ be a DFA defined by the 5-tuple $(Q, \Sigma, \delta_M, q_0, F)$. We construct the NFA $N$ as $(Q, \Sigma \cup \\{\epsilon\\}, \delta_N, q_0, F)$, where -->

<!-- $$ -->
<!-- \delta_N(q, a) = \{ \delta_M(q, a) \}. -->
<!-- $$ -->

<!-- Furthermore, let $\hat\delta_M$ and $\hat\delta_N$ be defined as in the above sections respectively. Now we must show that $L(M) = L(N)$ which, substituting the acceptance conditions for a string $w \in \Sigma^*$, reduces to: -->

<!-- $$ -->
<!-- \hat\delta_M(q_0, w) \in F \iff \hat\delta_N(q_0, w) \cap F \neq \emptyset. -->
<!-- $$ -->

<!-- This equivalence in itself is deducible from the stronger result -->

<!-- $$ -->
<!-- \hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \} -->
<!-- $$ -->

<!-- We prove the latter by induction on the length of $w$. -->

<!-- #### Basis: $w = \epsilon$ -->

<!-- Simply substituting is enough (note that $N$ by definition has no $\epsilon$ transitions). -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta_N(q_0, \epsilon) &= \Epsilon^*(q_0) \\ -->
<!-- &= \{q_0\} \\ -->
<!-- &= \{\hat\delta_M(q_0, \epsilon)\}. -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- Thus the equality holds. -->

<!-- #### Induction: $w\prime = wa$ -->

<!-- Suppose that $\hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \}$ holds. We now need to show that it also holds true for $w\prime$. By substitution we get -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta_N(q_0, w\prime) &= \hat\delta_N(q_0, wa) \\ -->
<!-- &= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \Epsilon^*(\delta_N(q_i, a)) \\ -->
<!-- &= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \Epsilon^*(\{\delta_M(q_i, a)\}) \\ -->
<!-- &= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \{\delta_M(q_i, a)\} \\ -->
<!-- &= \bigcup_{q_i \in \{\hat\delta_M(q_0, w)\}} \{\delta_M(q_i, a)\} \\ -->
<!-- &= \{ \delta_M(\delta_M(q_0, w), a) \} \\ -->
<!-- &= \{ \delta_M(q_0, wa) \} \\ -->
<!-- &= \{ \delta_M(q_0, w\prime) \} \\ -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- Thus the induction step is complete and therefore -->

<!-- $$ -->
<!-- \hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \} -->
<!-- $$ -->

<!-- holds true for any string $w \in \Sigma^*$. As stated earlier, this imples that $L(M) = L(N)$. -->

<!-- ### Any NFA can be represented as a DFA -->

<!-- Informally, we can show this by constructing a DFA where each state represents the collection of states that the original NFA can be in at any single point in time. For example, suppose that given an input symbol $a$, an NFA in state $q_1$ may transition to either $q_2$ or $q_3$. In our DFA, we would define a state $q_{23}$ which has an in-arrow for the input $a$ and represents the original NFA being in states $q_2$ or $q_3$ simultaneously. Since the number of states in a finite automaton is (obviously) finite, there are only a finite number (namely $ 2^{\|Q\|}) $ of possible "multi-states" that we can use for our DFA. Furthermore, we would define accepting states as those for which the original NFA is in at least one accepting state. -->

<!-- Below we present the formal proof. -->

<!-- --- -->

<!-- Let $N$ be an NFA defined by the 5-tuple $(Q, \Sigma \cup \\{ \epsilon \\}, \delta, q_0, F)$. We construct the DFA $M$ as $(Q\prime, \Sigma, \delta\prime, q_0\prime, F\prime)$ with an additional injective mapping $\phi: Q\prime \to \mathcal{P}(Q)$ which we call the *interpretation* of the states of $M$. Define the states of $M$ recursively as follows, -->

<!-- - $M$ has a state $q_0\prime$ with the interpretation $\phi(q_0\prime) = \Epsilon^*(\\{ q_0\\})$. -->
<!-- - Let $q\prime \in Q\prime$ and $a \in \Sigma$. Then $M$ has a state $q_i\prime$ with the interpretation -->

<!-- $$ -->
<!-- \phi(q_i\prime) = \bigcup_{q \in \phi(q\prime)} \Epsilon^*(\delta(q, a)). -->
<!-- $$ -->

<!-- Incidentally, this is also how we define $\delta\prime$, namely -->

<!-- $$ -->
<!-- \delta\prime(q\prime, a) = q_i\prime. -->
<!-- $$ -->

<!-- Lastly, we define the set of final states of $M$ as -->

<!-- $$ -->
<!-- F\prime = \{ q\prime \in Q \,|\, \phi(q\prime) \cap F \neq \emptyset \}. -->
<!-- $$ -->

<!-- Thus the acceptance condition for $M$ on the input $w \in \Sigma^*$ is expressed as -->

<!-- $$ -->
<!-- w \in L(M) \iff \hat\delta\prime(q_0\prime, w) \in F\prime \iff \phi(\hat\delta\prime(q_0\prime, w)) \cap F \neq \emptyset. -->
<!-- $$ -->

<!-- Now we must show that $L(M) = L(N)$ or -->

<!-- $$ -->
<!-- \hat\delta(q_0, w) \cap F \neq \emptyset \iff \hat\delta\prime(q_0, w) \in F\prime -->
<!-- $$ -->

<!-- which, using the above acceptance condition can be rewritten as -->

<!-- $$ -->
<!-- \hat\delta(q_0, w) \cap F \neq \emptyset \iff \phi(\hat\delta\prime(q_0\prime, w)) \cap F \neq \emptyset. -->
<!-- $$ -->

<!-- Again, we will actually prove the stronger result -->

<!-- $$ -->
<!-- \hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w)) -->
<!-- $$ -->

<!-- which implies the above equivalence. Similar to the proof that any DFA can be converted to an NFA, we use induction on the input string $w$. -->

<!-- #### Basis: $w = \epsilon$ -->

<!-- Subtitute directly to get -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta(q_0, \epsilon) &= \Epsilon^*(\{q_0\}) \\ -->
<!-- &= \phi(q_0\prime), -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- thus the base case holds. -->

<!-- #### Induction: $w\prime = wa$ -->

<!-- Suppose that $\hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w))$ holds and substitute, yielding -->

<!-- $$ -->
<!-- \begin{aligned} -->
<!-- \hat\delta(q_0, w\prime) &= \bigcup_{q \in \hat\delta(q_0, w)} \Epsilon^*{\delta(q, a)} \\ -->
<!-- &= \bigcup_{q \in \phi(\hat\delta\prime(q_0, w))} \Epsilon^*({\delta(q, a)}) \\ -->
<!-- &= \phi(\delta\prime(\hat\delta\prime(q_0, w), a)) \\ -->
<!-- &= \phi(\delta(q_0, wa)) \\ -->
<!-- &= \phi(\delta(q_0, w\prime)). -->
<!-- \end{aligned} -->
<!-- $$ -->

<!-- This completes the induction and thus we have proven that for any $w \in \Sigma^*$ -->

<!-- $$ -->
<!-- \hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w)), -->
<!-- $$ -->

<!-- which implies that the acceptance conditions are equivalent and therefore $L(M) = L(N)$. -->

<!-- {% include qed.html %} -->
