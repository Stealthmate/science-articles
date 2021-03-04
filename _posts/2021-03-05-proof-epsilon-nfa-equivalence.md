---
author: Valeri
title: A proof that DFA and NFA are equivalent
layout: post
toc: true
last_edit: 2021-03-05
abstract: I was giving a lecture on finite automata at a club gathering, and I had to prove that non-deterministic finite automata (with $\epsilon$ transitions) are equivalent to deterministic finite automata - a well known result. However, me being the lazy bloke I am, I didn't read the textbook beforehand and tried to prove this on the spot. Needless to say it didn't go as well as it should have, and I had to postpone the proof to the next gathering. I wanted to keep the formal proof for future reference, so I decided to write a short post about it. Feel free to quote this as you like (it should be correct, but do notify me if you spot a mistake). Since this is mainly a reference for myself, I'm not going to go into detail about what a finite automaton is, what it does, etc. However I will give the usual basic definitions so that the proof can be comprehensive.
---

## DFA in a nutshell

A DFA (short for *deterministic finite automaton*) is usually stated as a 5-tuple $(Q, \Sigma, \delta, q_0, F)$ where each element is defined as follows:

- $Q$ - the set of all possible states. Keep in mind, a "state" in this context is not a mathematical object. From the automaton's perspective, states need only be different from each other - it doesn't matter what they "are". From a human's perspective though, it is sometimes useful to give each state an interpretation, so that we understand what the automaton is doing in an abstract sense.
- $\Sigma$ - the set of possible inputs to the automaton. Also called the input alphabet.
- $\delta: Q\times\Sigma \to Q$ - the *state transition function*, that is a mapping which, given the current state and an input, evaluates to the next state after consuming the input.
- $q_0 \in Q$ - the initial state of the automaton.
- $F \subseteq Q$ - a subset of the possible states which are considered *final* (or *accepting*). Whether a state is accepting or not depends entirely on the interpretation of the problem.

This is enough to theoretically define an automaton, but we usually introduce another definition for convenience. The *extended transition function*

$$
\hat\delta : Q \times \Sigma^* \to Q,
$$

tells us the last state that the automaton enters after reading the whole input. Taking $\epsilon$ as the empty string, $w \in \Sigma^*$ and $a \in \Sigma$ we define $\hat\delta$ as

$$
\begin{aligned}
\hat\delta(q, \epsilon) &= q, \\
\hat\delta(q, wa) &= \delta(\hat\delta(w, q), a),
\end{aligned}
$$

If we call our automaton $M$ and denote the set of strings that are accepted by $M$ as $L(M)$, we can define the acceptance condition for a string $w \in \Sigma^*$ as

$$
w \in L(M) \overset{def}{\iff} \hat\delta(q_0, w) \in F.
$$

## NFA in a nutshell

A *non-deterministic automaton* (NFA for short) is like a DFA, except it can transition to multiple states (or none at all) for a given input. Thus the transition function is defined slithly differently

$$
\delta: Q \times \Sigma \to \mathcal{P}(Q).
$$

An top of this, we can also introduce $\epsilon$-transitions, that is, transitions which do not require any input ($\epsilon$ stands for the empty string, as denoted earlier). Such an NFA is given by the 5-tuple $(Q, \Sigma \cup \\{\epsilon\\}, \delta, q_0, F)$. Naturally, we also need to modify the extended transition function as well, but before that we need another concept.

We call

$$
\Epsilon_1^*: Q \to \mathcal{P}(Q)
$$

the $\epsilon$ *closure* of $q$ and define it as

$$
\Epsilon_1^*(q_1) = \{ q \in Q \,|\, \text{There is a path from } q_1 \text{ to } q \text{ that contains only } \epsilon \text{ transitions.}\}.
$$

We shall use this definition to define the closure for a set of states $S \subseteq Q$ as

$$
\Epsilon^*(S) = \bigcup_{q \in S} \Epsilon_1^*(q).
$$

We can now (re-)define the extended transition function:

$$
\begin{aligned}
\hat\delta : Q \times\Sigma^* &\to \mathcal{P}(Q) \\
(q, \epsilon) &\mapsto \Epsilon^*(\{ q \}) \\
(q, wa) &\mapsto \bigcup_{q_i \in \hat\delta(q, w)} \Epsilon^*(\delta(q_i, a)).
\end{aligned}
$$

The acceptance condition changes as well. A single input string can be resolved to multiple end states, but we define an NFA to accept a string if even a single resolution ends in a final state. Thus, we can state this property (for an NFA called $N$ and a string $w \in \Sigma^*$) as

$$
w \in L(N) \overset{def}{\iff} \hat\delta(q_0, w) \cap F \neq \emptyset.
$$

## The Proof

In order to show that NFA are equivalent, we must show that all DFAs can be represented as an NFA and vice versa.

### Any DFA can be represented as an NFA

Informally, since an NFA only allows non-deterministic transitions but doesn't require them, any DFA is trivially an NFA. This in itself should be convincing enough, but we still provide a formal proof.

Let $M$ be a DFA defined by the 5-tuple $(Q, \Sigma, \delta_M, q_0, F)$. We construct the NFA $N$ as $(Q, \Sigma \cup \\{\epsilon\\}, \delta_N, q_0, F)$, where

$$
\delta_N(q, a) = \{ \delta_M(q, a) \}.
$$

Furthermore, let $\hat\delta_M$ and $\hat\delta_N$ be defined as in the above sections respectively. Now we must show that $L(M) = L(N)$ which, substituting the acceptance conditions for a string $w \in \Sigma^*$, reduces to:

$$
\hat\delta_M(q_0, w) \in F \iff \hat\delta_N(q_0, w) \cap F \neq \emptyset.
$$

This equivalence in itself is deducible from the stronger result

$$
\hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \}
$$

We prove the latter by induction on the length of $w$.

#### Basis: $w = \epsilon$

Simply substituting is enough (note that $N$ by definition has no $\epsilon$ transitions).

$$
\begin{aligned}
\hat\delta_N(q_0, \epsilon) &= \Epsilon^*(q_0) \\
&= \{q_0\} \\
&= \{\hat\delta_M(q_0, \epsilon)\}.
\end{aligned}
$$

Thus the equality holds.

#### Induction: $w\prime = wa$

Suppose that $\hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \}$ holds. We now need to show that it also holds true for $w\prime$. By substitution we get

$$
\begin{aligned}
\hat\delta_N(q_0, w\prime) &= \hat\delta_N(q_0, wa) \\
&= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \Epsilon^*(\delta_N(q_i, a)) \\
&= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \Epsilon^*(\{\delta_M(q_i, a)\}) \\
&= \bigcup_{q_i \in \hat\delta_N(q_0, w)} \{\delta_M(q_i, a)\} \\
&= \bigcup_{q_i \in \{\hat\delta_M(q_0, w)\}} \{\delta_M(q_i, a)\} \\
&= \{ \delta_M(\delta_M(q_0, w), a) \} \\
&= \{ \delta_M(q_0, wa) \} \\
&= \{ \delta_M(q_0, w\prime) \} \\
\end{aligned}
$$

Thus the induction step is complete and therefore

$$
\hat\delta_N(q_0, w) = \{ \hat\delta_M(q_0, w) \}
$$

holds true for any string $w \in \Sigma^*$. As stated earlier, this imples that $L(M) = L(N)$.

### Any NFA can be represented as a DFA

Informally, we can show this by constructing a DFA where each state represents the collection of states that the original NFA can be in at any single point in time. For example, suppose that given an input symbol $a$, an NFA in state $q_1$ may transition to either $q_2$ or $q_3$. In our DFA, we would define a state $q_{23}$ which has an in-arrow for the input $a$ and represents the original NFA being in states $q_2$ or $q_3$ simultaneously. Since the number of states in a finite automaton is (obviously) finite, there are only a finite number (namely $ 2^{\|Q\|}) $ of possible "multi-states" that we can use for our DFA. Furthermore, we would define accepting states as those for which the original NFA is in at least one accepting state.

Below we present the formal proof.

---

Let $N$ be an NFA defined by the 5-tuple $(Q, \Sigma \cup \\{ \epsilon \\}, \delta, q_0, F)$. We construct the DFA $M$ as $(Q\prime, \Sigma, \delta\prime, q_0\prime, F\prime)$ with an additional injective mapping $\phi: Q\prime \to \mathcal{P}(Q)$ which we call the *interpretation* of the states of $M$. Define the states of $M$ recursively as follows,

- $M$ has a state $q_0\prime$ with the interpretation $\phi(q_0\prime) = \Epsilon^*(\\{ q_0\\})$.
- Let $q\prime \in Q\prime$ and $a \in \Sigma$. Then $M$ has a state $q_i\prime$ with the interpretation

$$
\phi(q_i\prime) = \bigcup_{q \in \phi(q\prime)} \Epsilon^*(\delta(q, a)).
$$

Incidentally, this is also how we define $\delta\prime$, namely

$$
\delta\prime(q\prime, a) = q_i\prime.
$$

Lastly, we define the set of final states of $M$ as

$$
F\prime = \{ q\prime \in Q \,|\, \phi(q\prime) \cap F \neq \emptyset \}.
$$

Thus the acceptance condition for $M$ on the input $w \in \Sigma^*$ is expressed as

$$
w \in L(M) \iff \hat\delta\prime(q_0\prime, w) \in F\prime \iff \phi(\hat\delta\prime(q_0\prime, w)) \cap F \neq \emptyset.
$$

Now we must show that $L(M) = L(N)$ or

$$
\hat\delta(q_0, w) \cap F \neq \emptyset \iff \hat\delta\prime(q_0, w) \in F\prime
$$

which, using the above acceptance condition can be rewritten as

$$
\hat\delta(q_0, w) \cap F \neq \emptyset \iff \phi(\hat\delta\prime(q_0\prime, w)) \cap F \neq \emptyset.
$$

Again, we will actually prove the stronger result

$$
\hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w))
$$

which implies the above equivalence. Similar to the proof that any DFA can be converted to an NFA, we use induction on the input string $w$.

#### Basis: $w = \epsilon$

Subtitute directly to get

$$
\begin{aligned}
\hat\delta(q_0, \epsilon) &= \Epsilon^*(\{q_0\}) \\
&= \phi(q_0\prime),
\end{aligned}
$$

thus the base case holds.

#### Induction: $w\prime = wa$

Suppose that $\hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w))$ holds and substitute, yielding

$$
\begin{aligned}
\hat\delta(q_0, w\prime) &= \bigcup_{q \in \hat\delta(q_0, w)} \Epsilon^*{\delta(q, a)} \\
&= \bigcup_{q \in \phi(\hat\delta\prime(q_0, w))} \Epsilon^*({\delta(q, a)}) \\
&= \phi(\delta\prime(\hat\delta\prime(q_0, w), a)) \\
&= \phi(\delta(q_0, wa)) \\
&= \phi(\delta(q_0, w\prime)).
\end{aligned}
$$

This completes the induction and thus we have proven that for any $w \in \Sigma^*$

$$
\hat\delta(q_0, w) = \phi(\hat\delta\prime(q_0\prime, w)),
$$

which implies that the acceptance conditions are equivalent and therefore $L(M) = L(N)$.

{% include qed.html %}
