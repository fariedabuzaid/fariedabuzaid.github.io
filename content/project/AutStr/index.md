---
date: 2026-07-07
links:
  - type: code
    url: https://github.com/fariedabuzaid/AutStr
title: "AutStr: Symbolic Infinite Structures in Python"
authors:
  - Faried Abu Zaid
featured: true
image:
  caption: 'Image credit: ChatGPT'
  focal_point: ""
  preview_only: false
tags:
  - Automatic Structures
  - Linear Integer Arithmetic
  - Symbolic Computation
  - First Order Logic
  - Monadic Second Order Logic
  - Model Theory
---

**AutStr** is a Python library for symbolic representation and manipulation of
infinite relational structures. It represents infinite mathematical objects — the
integers ℤ, the localizations ℤ[1/p], whole classes of finite graphs and groups —
as **finite automata**, and lets you query them with first-order and monadic
second-order logic. Because the representation is exact and the logic is decidable,
a single small framework acts as several tools at once:

- 🧮 **a computer algebra system** for infinite domains — exact algebra over
  infinite sets and relations, not floating point;
- ⊢ **a decision procedure / theorem prover** — decide first-order and MSO
  statements over infinite structures (Presburger and Büchi arithmetic, MSO over
  graphs), returning a proof-carrying yes/no;
- 🔬 **a finite algebra & model-theory system** — decide a property across an
  *entire family* of finite structures with one compiled automaton;
- ⚙️ **an algorithm synthesizer** — turn a logical *specification* into a
  **provably linear-time algorithm**.

All four are the same underlying object — an *automatic presentation* — viewed
from different angles.

> **Version 2.0** (July 2026) adds *uniformly automatic classes* for whole families
> of finite structures, and a rewritten batched-NumPy automata core that is
> **10²–10³× faster** than v1, with JAX as an optional accelerator. See the
> [release announcement](/post/autstr-v2/).

## Installation

```bash
pip install autstr              # NumPy-only core — installs anywhere
pip install autstr[jax]         # + JAX-accelerated batch word processing
pip install autstr[graphs]      # + networkx conversion for the graph classes
```

Requires Python 3.10–3.14.

## Synthesizing linear-time algorithms

Write *what* you want as a logical formula; AutStr compiles it — once — into a
finite automaton that decides it. On structurally restricted inputs (bounded
tree-depth, bounded pathwidth) that automaton is a **linear-time algorithm**, even
for properties that are NP-hard in general — a constructive, streaming form of
Courcelle's theorem.

```python
import networkx as nx
from autstr.graphs import TreeDepthClass, TreeDepthGraph

cls = TreeDepthClass(3)

# Bipartiteness as a monadic second-order formula, compiled ONCE for the whole
# class into a 6-state automaton:
bipartite, _ = cls.evaluate(
    'exists c.(all x.(all y.((not E(x,y)) or '
    '((Subset(x,c) and (not Subset(y,c))) or '
    '((not Subset(x,c)) and Subset(y,c))))))')

triangle = TreeDepthGraph.from_networkx(nx.cycle_graph(3))
bipartite.accepts([(s,) for s in cls.advice(triangle)])   # False — in microseconds
```

Deciding the property on a graph is a single linear pass, and the work batches
beautifully (optionally on a GPU via the JAX backend):

![Linear-time MSO query evaluation on bounded tree-depth graphs](runtime_curves.svg)

- **Perfectly linear** decision time (through-the-origin fit, R² = 1.0000); the JAX
  backend decides a **million-vertex graph in ~20 ms** per query.
- **Batched evaluation** classifies tens of thousands of graphs at once at
  **~90 million vertices / second** — about **190×** a naive per-graph loop.

## Programming with infinite sets

Because relations are first-class infinite objects, you can also write algorithms
that manipulate them directly. Here is the Sieve of Eratosthenes running over the
**actual infinite set** of integers — no bound, no array:

```python
from autstr.arithmetic import VariableETerm as Var

x = Var('x')

def infinite_sieve(steps):
    candidates = x.gt(1)                          # the infinite set {2, 3, 4, ...}
    primes = []
    for _ in range(steps):
        for (p,) in candidates:                   # enumerates smallest-first
            primes.append(p); break               # ... so this is the next prime
        multiples = (x.eq(primes[-1] * Var('y'))).drop(['y'])
        candidates = candidates & ~multiples      # remove its multiples, symbolically
    return primes, candidates

primes, remaining = infinite_sieve(4)
# primes    == [2, 3, 5, 7]
# remaining  is the infinite set enumerating 11, 13, 17, 19, 23, 29, ...
```

## Uniformly automatic classes

New in version 2, a **uniformly automatic class** presents an entire family of
finite structures by giving every automaton an extra tape that reads an *advice
string*. A query is compiled once for the class and then decides any member.
Built-in classes cover:

| package | classes | signature |
|---------|---------|-----------|
| `autstr.graphs`  | bounded **tree-depth**, bounded **pathwidth** | full MSO over vertex sets |
| `autstr.algebra` | finite **Boolean algebras**, finite **abelian groups**, **ℤ[1/p]** | `Meet`/`Join`/`Compl`/`Leq`/`Atom`; `+` |
| `autstr.groups`  | **index-≤2 cyclic** groups (dihedral, quaternion, semidihedral, modular), **extraspecial** p-groups | multiplication `M` |

## References
  - Abu Zaid, F. *Algorithmic Solutions via Model Theoretic Interpretations.* Dissertation, RWTH Aachen University, 2016. [DOI](https://doi.org/10.18154/RWTH-2017-07663)
  - Abu Zaid, F. *Uniformly Automatic Classes of Finite Structures.* FSTTCS 2018. [DOI](https://doi.org/10.4230/LIPIcs.FSTTCS.2018.10)
  - Abu Zaid, F., Grädel, E., & Reinhardt, F. *Advice Automatic Structures and Uniformly Automatic Classes.* CSL 2017. [DOI](https://doi.org/10.4230/LIPIcs.CSL.2017.35)
  - Blumensath, A., & Grädel, E. *Automatic Structures.* LICS 2000. [URL](https://lics.siglog.org/2000/Grdel-AutomaticStructures.html)
  - Khoussainov, B., & Nerode, A. *Automatic presentations of structures.* LCC 1994. [DOI](https://doi.org/10.1007/3-540-60178-3_93)
  - Khoussainov, B., Rubin, S., & Stephan, F. *Automatic Structures: Richness and Limitations.* LMCS 2007. [arXiv](https://arxiv.org/abs/cs/0703064) [DOI](https://doi.org/10.2168/LMCS-3%282%3A2%292007)
