<!-- solve-problem:d275fbfd-5143-4016-b3bf-fd30cdaa9b41 -->
# Deterministic FPTAS for the nonnegative permanent

A [Tangle Solve problem](https://solve.tangle.tools/problems/d275fbfd-5143-4016-b3bf-fd30cdaa9b41).

## What is the problem?

Given an n×n matrix A with entries in ℚ≥0 and an error parameter ε > 0, compute deterministically a value P̂ such that (1−ε)·per(A) ≤ P̂ ≤ (1+ε)·per(A), in time poly(n, 1/ε) (an FPTAS). Here per(A) = Σ_{σ∈S_n} ∏_i A_{i,σ(i)} counts perfect matchings in the associated bipartite graph with edge weights. The input size includes the bit length of the entries. The output need only be a number, not a sample. Randomized algorithms are excluded: no random bits, no Las Vegas/Monte Carlo behavior — the guarantee must hold on every input. Note the subtlety that per(A) can be doubly exponentially small in bit-complexity terms for rational inputs, so "relative error" interacts delicately with output precision; a reasonable formalization requires output as a rational with poly bit length, and one should check whether the intended formulation assumes integer entries (where per(A) ≥ 1 when nonzero) — the integer-entry version is the canonical interpretation, with the rational case following by scaling.

## Why does it matter?

The permanent is the canonical #P-complete counting problem (Valiant 1979), and the Jerrum–Sinclair–Vigoda (JSV) algorithm is the landmark result showing #P-hard counting problems admit randomized FPTASs. A deterministic FPTAS would be the first full derandomization of a nontrivial #P-complete counting problem, resolving a central open question in approximate counting. It would also likely yield deterministic algorithms for related partition functions (monomer-dimer, matchings in physics models) and would advance the broader program of connecting counting complexity to circuit lower bounds (via the hardness-vs-randomness connection for counting problems).

## What's already known?

- JSV (2004) gives a randomized FPTAS using simulated annealing on a Markov chain over matchings of a graph pair (G, H) with a weighting parameter z; the chain mixes rapidly for all z, enabling cooling from an easy regime to the target. Randomness enters through sampling the Markov chain.
- For 0/1 matrices, Gurvits's "van der Waarden/glynn" type spectral approaches give randomized or specialized results: Gurvits (2011) gave a randomized poly-time algorithm for permanents of matrices with PSD comparison properties (verify details). There is a deterministic poly(n, 1/ε) algorithm achieving additive error via Bethe/Gauss-approximation (Gurvits–Samorodnitsky) and an e^n-approximation ratio.
- Deterministic exact algorithms are exponential (#P-hardness forbids poly time unless P=#P).
- For restricted classes (bounded rank, bounded treewidth, doubly stochastic structure partial results), deterministic approximations exist.
- General deterministic approximation with ratio better than e^{n^{1−δ}} is not known to be #P-hard, so no complexity barrier currently blocks an FPTAS — the obstacle is purely algorithmic: no deterministic substitute for Markov chain sampling is known at the necessary accuracy.

Key approaches tried: determinantal point processes (fail — they approximate the determinant-like structure, not permanent), LP/entropy relaxations (give lower bounds only), derandomizing the JSV random walk via pseudorandomness (walks on exponentially large state spaces resist known PRG constructions with the needed seed length), and Barbier-style rejection sampling variants.

## What would count as solving it?

A deterministic algorithm M(A, ε) running in poly(n, 1/ε, bit-length) time such that on every nonnegative matrix A, M outputs P̂ with (1−ε)per(A) ≤ P̂ ≤ (1+ε)per(A). Partial credits worth listing:

1. **Full solution:** deterministic FPTAS for all nonnegative matrices.
2. **0/1 case:** deterministic FPTAS for perfect-matching counts of bipartite graphs (already major).
3. **Weaker ratio:** deterministic n^c- or e^{o(n)}-factor approximation would be substantial progress.
4. **Hardness:** proving #P-hardness of deterministic constant-factor approximation (this would close the problem negatively).

## How to start

1. Read Jerrum–Sinclair–Vigoda (JACM 2004) thoroughly, especially the annealing schedule and the weight function w(G,H,z); the entire analysis rests on sampling from specific distributions, so catalog exactly where randomness is used.
2. Read Gurvits's works on permanent approximation (e.g., "On multivariate Newton-like inequalities" and Gurvits–Samorodnitsky on the Bethe approximation) to understand the best deterministic bounds (e^n ratio) and where entropy relaxation loses accuracy.
3. Consider the conditional route: known PRGs fooling space-bounded computation give deterministic approximate counting for DLOGTIME-type classes; the JSV chain is a random walk on an exponentially large graph with polynomial-bounded "local" structure — investigate whether state-space-fooling walks (e.g., REingold-style zig-zag / pseudorandom walk generators) can be applied, and identify the seed-length barrier.
4. Try the easier monomer-dimer problem first (deterministic FPTAS for the total matching polynomial is also open but more tractable) as a warm-up.

Main obstacle: the state space is exponential and the distributions sampled change continuously during annealing; standard PRG seed lengths (log-space generators) blow up polynomially per step times steps, which is fine, but no known generator provably fools the specific acceptance statistics.

## Key references

- M. Jerrum, A. Sinclair, E. Vigoda, "A Polynomial-Time Approximation Algorithm for the Permanent of a Matrix with Nonnegative Entries," JACM 51(4), 2004. The randomized FPTAS via simulated annealing on matchings; the target of derandomization.
- L. Valiant, "The Complexity of Computing the Permanent," Theoretical Computer Science 8, 1979. Establishes #P-completeness of the 0/1 permanent.
- L. Gurvits, "On multivariate Newton-like inequalities," Advances in Combinatorial Mathematics, 2010 (verify venue/year). Gives deterministic capacity-based lower/upper bounds for permanents, yielding exponential-ratio approximations.
- L. Gurvits, A. Samorodnitsky, "Bounds on the permanent…" (approximation via Bethe entropy / capacity), 2014 (verify). Proves the Bethe approximation is within e^n deterministically.
- A. Sinclair, "Algorithms for Random Generation and Counting: A Markov Chain Approach," Birkhäuser 1993. Background on the Markov chain Monte Carlo counting framework underlying JSV.

## Warnings

- "Nonnegative" includes rationals with tiny entries; be careful that relative-error guarantees with poly-bit output are even well-defined — restrict to integer entries first.
- Randomized FPTAS ≠ deterministic: any construction using samplers, hash-to-randomness, or Monte Carlo steps does not solve this even with one-sided error.
- Do not confuse with the additive-error or e^n-multiplicative deterministic results already known; those do not give FPTAS.
- The related problem COMB-000211 is stated as identical (same-as); check for duplicate prior work there before claiming novelty.
- Derandomization attempts via naive seed-length accounting fail because the Markov chain's transition function must be efficiently computable for the PRG composition; JSV's transitions involve rapidly mixing counts, not just neighbor queries — this subtlety sinks most first attempts.

---
*`ALGO-000140` · unverified*

## Acceptance guidance

A deterministic algorithm M(A, ε) running in poly(n, 1/ε, bit-length) time such that on every nonnegative matrix A, M outputs P̂ with (1−ε)per(A) ≤ P̂ ≤ (1+ε)per(A). Partial credits worth listing:

1. **Full solution:** deterministic FPTAS for all nonnegative matrices.
2. **0/1 case:** deterministic FPTAS for perfect-matching counts of bipartite graphs (already major).
3. **Weaker ratio:** deterministic n^c- or e^{o(n)}-factor approximation would be substantial progress.
4. **Hardness:** proving #P-hardness of deterministic constant-factor approximation (this would close the problem negatively).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/d275fbfd-5143-4016-b3bf-fd30cdaa9b41?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000140.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000140)
