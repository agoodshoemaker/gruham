---
hide:
  - navigation
---

# Stable Matching Problem

Notes gathered from studying the stable matching problem.

## N1: Definitions

$M = \{m_1, m_2, \dots, m_n\}$ and $W = \{w_1, w_2, \dots, w_n\}$ represent the set of men and women respectively. There are $n$ men and an equal number of women: $|M| = |W| = n$.

<u>Matching</u>

A matching is a subset of $M \times W$ in which each man and woman appear at most one time in a pair.

<u>Perfect Matching</u>

A perfect matching is a matching in which each man and woman appear in exactly one pair. There is neither polygamy nor singlehood. The size of a perfect matching is $n$.

<u>Instability</u>

Let $S$ be a perfect matching. $(m, w)$ and $(m^{\prime}, w^{\prime})$ are two pairs that belong to $S$. If $m$ prefers $w^{\prime}$ to $w$ and $w^{\prime}$ prefers $m$ to $m^{\prime}$, then $(m, w^{\prime})$ is termed an instability with respect to $S$.

<u>Stable Matching</u>

A perfect matching $S$ is stable if there is no instability with respect to it. Stable matchings are desirable as they are self-enforcing. In a stable matching no man will be able to abandon his current partner for some other woman higher up on his preference list. The same is true for women. A stable matching represents some sort of an equilibrium.

<u>Valid partner</u>

A woman $w$ is a valid partner of a man $m$ if $(m, w)$ is present in some stable matching.

<u>Best valid partner</u>

Let $w$ be a valid partner of $m$. If $m$ prefers $w$ over every one of his valid partners, then $w$ is said to be the best valid partner of $m$. 

Interesting questions:

- Can a perfect matching be assigned a potential energy? If that can be done, then a stable matching must be a configuration having the least potential energy.
- The preference lists of all women can be collated to give a score to each man. We could call this the desirability of each man. Likewise, we could compute a desirability score for each woman.
- What happens if we pair the most desirable man with the most desirable woman? Would that result in a stable matching?
- Tried this approach. This doesn't work. Though it seems like a good idea at first sight, this process ends up conflating the preference lists. Conflation causes confusion here.



## N2: Combinatorial aspects

Some basic combinatorial results:

- The space of ordered pairs is represented by $M \times W$. There are a total of $n^2$ ordered pairs possible. Each ordered pair is of the form $(m, w)$.
- The number of perfect matchings is $n!$
- There are $2n$ preference lists, each of size $n$.
- $(n!)^{2n}$ is the size of the problem space. Each element in this space corresponds to one instantiation of the problem.

Some interesting questions that have a combinatorial flavour:

- How many matchings are possible?
- Can we determine the number of stable matchings? 
- What proportion of perfect matchings are stable? 



## N3: Problem

Given a set of arbitrary preferences, the following questions can be asked:

- Existence: Does a stable matching exist?
- Uniqueness: If it exists, is it unique?
- Construction: Is there an algorithm to construct a stable matching?
- Efficient construction: If there is an efficient algorithm to construct a stable matching?

A stable matching is an object. An object is some abstract but well defined entity. Mathematics usually concerns itself with the first two points: existence and uniqueness of an object. Theoretical computer science is more interested in the last two points. An efficient algorithm in some sense is the crown of the whole thought process as it englobes all other stages in this process, with the exception of uniqueness.



## N4: Gale-Shapley algorithm

The basic idea:

- All men and women are free to begin with. 
- At any stage in the process, a man $m$ could either be free or engaged. If $m$ is free, then he can propose to a woman. $m$ proposes to each woman at most once. This is expected behaviour.
- Naturally, he will propose to a woman highest in his preference list to whom he hasn't already proposed, say $w$. Now, $w$ could either be free or engaged. If she is free, then $(m, w)$ get engaged. If $w$ is engaged to some other man, say $m^{\prime}$, we have two situations:
  - if $w$ prefers her current partner over $m$, then she will refuse the proposal from $m$
  - if $w$ prefers $m$ to her current partner, then she will abandon $m^{\prime}$ and get engaged with $m$.
- Keep repeating the process until there are no free men.

The core of the algorithm can be summarised in this sentence:

> In each iteration of the algorithm a free man proposes to a woman he hasn't already proposed to in the decreasing order of his preference list.

<u>Key features</u>

- Once a woman is engaged, she always stays engaged.
- Men may enter and exit out of engagements.
- The sequence of partners for a man decreases.
- The sequence of partners for a woman increases.
- The number of free men is equal to the number of free women at the beginning and end of every step of the iteration. This equality is therefore an invariant.
- If a man is free, it implies that a woman is free.
- If a woman is free, it implies that no one has proposed to her yet.

<u>Termination</u>

In each step of the iteration, we are going exactly one step down the preference list of some man. So, after at most $n^{2}$ iterations, we must have gone down the preference list of all $n$ men. If at this stage, there is a free man, then it implies that there is also a free woman. But the free man would have proposed to this free woman at some stage. In which case, she can never be free. A contradiction that there can be a free man! So, the algorithm terminates and returns a perfect matching.

<u>Stable Matching</u>

Let $(m, w)$ and $(m^{\prime}, w^{\prime})$ be two pairs in a perfect matching $S$ returned by the GS algorithm. Contrary to the result we have to prove, let us assume that $(m, w^{\prime})$ constitutes an instability with respect to $S$. Since $m$ prefers $w^{\prime}$ to $w$, he would have proposed to $w^{\prime}$ at some stage in the algorithm. Clearly, $w^{\prime}$ rejected him. However, she ends up with $m^{\prime}$ who is lower than $m$ on her preference list. This cannot be and is a contradiction!

Interesting questions:

- We can define a "happiness score" for each stable matching. For each man and woman, we ask them how happy they are with their current partner. This depends on the preference list of each person. We could assign a linear function over the preference list. Once the score is defined, we can average over all scores of the $2n$ people. This represents the "happiness score" of a stable matching. What can we say about the "happiness score" of the matching returned by the GS algorithm? Does the GS algorithm maximize the happiness score? Can we think of algorithms that optimize for this happiness score?



## N5: Stable Matchings

Consider the simple setup of $n = 2$:


$$
\begin{align}
M &= \{m_1, m_2\}\\
W &= \{w_1, w_2\}
\end{align}
$$


<u>Configuration-1</u>


$$
\begin{align}
m_1 &: [w_1, w_2]\\ m_2 &: [w_2, w_1]\\ w_1 &: [m_1, m_2]\\w_2 &: [m_2, m_1]\end{align}
$$



There is only one stable matching here: $\{(m_1, w_1), (m_2, w_2)\}$

Note how the preferences of the men mesh perfectly with each other. Likewise, the preferences of the women mesh perfectly with each other. This harmony is there across the two genders as well. 

> This represents an ideal world in which everyone is happy as one could possibly be.

<u>Configuration-2</u>


$$
\begin{align}
m_1 &: [w_1, w_2]\\ m_2 &: [w_2, w_1]\\ w_1 &: [m_2, m_1]\\w_2 &: [m_1, m_2]\end{align}
$$


$\{(m_1, w_1), (m_2, w_2)\}$ is a stable matching. Here, the preferences of men mesh perfectly with each other, that of women also are in harmony. But, there is no harmony across the genders. So, while the men are perfectly happy, the women are miserable.



$\{(m_1, w_2), (m_2, w_1)\}$ is another stable matching. Here, women are happy and men are miserable. This configuration shows that stable matchings are not unique. Not just that, there is an asymmetry in the kind of matchings that we obtain.

> This represents a divided world in which one group is happy and the other group is miserable.



<u>Configuration-3</u>


$$
\begin{align}
m_1 &: [w_1, w_2]\\ m_2 &: [w_1, w_2]\\ w_1 &: [m_2, m_1]\\w_2 &: [m_2, m_1]\end{align}
$$



$\{(m_1, w_1), (m_2, w_2)\}$ is not a stable matching as $(m_2, w_1)$ is an instability. $\{(m_1, w_2), (m_2, w_1)\}$ has to be a stable matching. This is an interesting matching. There is perfect compatibility as far as one pair is concerned, and total incompatibility in case of the other pair.

> This represents a more complex world in which some pairs are happy and others are miserable. But at the level of the group, the outcome is somewhat the same.

<u>Interesting configurations</u>

- What happens to a configuration in which a man is at the bottom of the preference list of every woman?
- What happens if the preference list of all men are identical?



## N6: Consistency

For a given configuration, the stable matching returned by the GS algorithm is always the same and is given by:


$$
S^{*} = \{(m, best(m))\ |\ m \in M\}
$$


where $best(m)$ is the best valid partner of $m$. Interesting questions:

- Is this even a matching? Two men could have the same best valid partner.
- Is this a perfect matching?
- Is this a stable matching?

The basic idea is to posit a stable matching returned by GS where a man $m$ has been rejected by his best a valid partnter. Let us proceed with this idea. Assume that $S$ is a stable matching returned by GS in which a man $m$ is rejected by one of his valid partners, say $w$, for the very first time in the execution of the algorithm. Since men propose in the decreasing order of preferences, $w$ has to be the best valid partner of $m$.

Now we focus on this rejection. What happened when $w$ rejected $m$? One of these two things could have happened:

- $w$ was already engaged to a man $m^{\prime}$ whom she prefers to $m$.
- $w$ was engaged to $m$, but a new proposal from a man $m^{\prime}$ whom she prefers to $m$ made her abandon $m$

In either case, we see that $w$ prefers $m^{\prime}$ over $m$. Now, there is a stable matching $S^{\prime}$ in which $(m, w)$ is a pair. This is because $w$ is a valid (best) partner of $m$. In this matching, who is $m^{\prime}$ paired with? Let us say that it is a woman $w^{\prime}$. Clearly, $w^{\prime}$ is a valid partner of $m^{\prime}$. Now, $m^{\prime}$ prefers $w$ over $w^{\prime}$. If not, then in our algorithm, $m^{\prime}$ would have rejected $w^{\prime}$, one of his valid partners, before getting engaged to $w$. But this cannot be as we have assumed that $w$ is the first woman to reject a valid partner.

We now have the following preferences:

- $w$ prefers $m^{\prime}$ over $m$
- $m^{\prime}$ prefers $w$ over $w^{\prime}$

In the stable matching $S^{\prime}$, we have the pairs $(m, w)$ and $(m^{\prime}, w^{\prime})$. With respect to $S^{\prime}$, $(m^{\prime}, w)$ constitutes an instability. And that contradicts our initial claim.



## N7: Asymmetry and philosophical implications

The asymmetry in the GS algorithm is that the side that does the proposing ends up with the best outcomes and the side that accepts and evaluates the proposals ends up with the worst possible outcomes, with the terms best and worst understood in a precise sense.

Let us call the side that proposes the **active** side and the side that accepts proposals the **passive** side. We see that the active side is better off than the passive side. Notice that it appears as though the passive side is actually better off. As the algorithm progresses, passive participants seem to be in a superior state. What gives this impression? The active participants come to the passive folks with proposals, the passive participants seem to have the power of rejection. The active participants can only propose and hope for the best. It appears as though they don't have any agency and power in the process. So, how is it that the passive side is worse off in the end?

The power of rejection is an illusion or is a misreading. The passive participants do not reject intelligently, they reject on principle, myopically. They suffer from what is nowadays called FOMO: fear of missing out. This is again the reason why the accept the first proposal that comes their way. On top of this, the passive participants lack initiative. The other problem with the passive participants is that they are opportunistic. The moment a better proposal comes, they abandon the current engagement. So, the passive groups can be characterized as:

- myopic
- opportunistic

The active participants on the other hand are:

- loyal
- enterprising

This characterisation of the two groups is an emergent property of the algorithm. The algorithm's design is responsible for this characterisation. A natural question then is, can we invert this process? That is, can we design an algorithm that will conform to an a priori characterisation of the groups. So, what if we want an algorithm in which both groups are enterprising? Can we design an algorithm in which the emergent property is in line with this seed idea?



## N8: Other ideas

Can we randomly pair each man to a woman and then keep looking for instabilities and remove them one by one? Will this result in a stable matching? Surprisingly, this does seem to work!

- A simple random pairing is as follows: $S = \{(m_i, w_i)\ |\ 1 \leq i \leq n\}$
- A simple nested loop goes over all pairs of pairs and checks for an instability. If there is one, it removes it.
- This keeps happening until the matching stabilises.

What is interesting is:

- GS and this randomised algorithm return the same stable matching several times for small values of $n$
- There are cases when this algorithm returns a different stable matching.
- It is obviously inefficient as it is currently implemented.
- There are cases when algorithm goes into an infinite loop. Is it some kind of a local optimum?
- The number of iterations before it stabilises is huge. For example, when $n = 20$, it is $80$ in one case and $20000$ in another.

This question has already been resolved in the paper by Roth and Vate: Random Paths to Stability in Two-Sided Matching. Also refer to [this](https://arxiv.org/pdf/2007.07121.pdf).
