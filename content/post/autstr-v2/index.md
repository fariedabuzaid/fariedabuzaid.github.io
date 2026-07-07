---
title: "AutStr 2.0 — Uniformly Automatic Classes and Linear-Time Algorithm Synthesis"
summary: A major rewrite of AutStr — a 10²–10³× faster automata core, uniformly automatic classes for whole families of finite structures, and MSO queries that compile into provably linear-time algorithms.
date: 2026-07-07

authors:
  - admin

tags:
  - Academic
  - Project
  - Release
  - Automatic Structures
  - Model Theory

image:
  caption: 'Linear-time MSO query evaluation on graphs of bounded tree-depth (AutStr benchmark suite)'
  focal_point: ""
  preview_only: false

content_meta:
  trending: true
---

I'm excited to announce **AutStr 2.0**, a major release of the Python library for
symbolic computation over infinite mathematical structures using automata theory.
Version 2 is both a large capability jump and a near-complete rewrite of the
engine underneath.

`pip install autstr` · GitHub: [fariedabuzaid/AutStr](https://github.com/fariedabuzaid/AutStr)

## One formalism, four roles

AutStr represents infinite structures — the integers ℤ, the localizations ℤ[1/p],
whole classes of finite graphs and groups — as **finite automata**, and lets you
query them with first-order and monadic second-order logic. Because the
representation is exact and the logic is decidable, a single small framework acts
as several tools at once:

- 🧮 **a computer algebra system** for infinite domains — manipulate infinite sets
  and relations with exact algebra, not floating point;
- ⊢ **a decision procedure / theorem prover** — decide first-order and MSO
  statements over infinite structures (Presburger and Büchi arithmetic, MSO over
  graphs), returning a proof-carrying yes/no;
- 🔬 **a finite algebra & model-theory system** — decide a property across an
  *entire family* of finite structures (all finite abelian groups, all graphs of
  bounded tree-depth) with one compiled automaton;
- ⚙️ **an algorithm synthesizer** — turn a logical *specification* into a
  **provably linear-time algorithm**.

All four are the same underlying object — an *automatic presentation* — viewed
from different angles.

## What's new in 2.0

**⚙️ Uniformly automatic classes.** The centrepiece of this release. A uniformly
automatic class presents not one structure but an entire *family*, by giving every
automaton an extra tape that reads an *advice string* synchronously with the
elements. A query is compiled once for the class and then decides any member.
Ready-made classes ship for:

- graphs of bounded **tree-depth** and **pathwidth**, with full MSO over vertex sets;
- finite **Boolean algebras**, finite **abelian groups**, and the **ℤ[1/p]** localizations;
- non-abelian groups — **dihedral, quaternion, semidihedral, modular**, and **extraspecial** p-groups.

**🚀 A rewritten automata core.** The entire engine is now a batched, sparsity-aware
NumPy implementation of sparse DFAs — a **10²–10³× speedup** over v1 (the reference
query dropped from 85 s to 0.03 s), with linear memory. JAX moved from a hard
dependency to an optional accelerator for bulk word processing.

**📐 Algorithm synthesis in practice.** Write *what* you want as a logical formula;
AutStr compiles it once into an automaton that decides it. On structurally
restricted inputs (bounded tree-depth, bounded pathwidth) that automaton is a
**linear-time algorithm** — even for properties that are NP-hard in general, such
as 3-colourability. This is a constructive, streaming form of Courcelle's theorem.

![Linear-time MSO query evaluation on bounded tree-depth graphs](runtime_curves.svg)

The benchmark suite (reproducible in [`benchmarks/`](https://github.com/fariedabuzaid/AutStr/tree/main/benchmarks))
measures this directly:

- **Perfectly linear** decision time — a through-the-origin fit of R² = 1.0000
  across three orders of magnitude; the JAX backend decides a **million-vertex
  graph in ~20 ms** per query.
- **Batched evaluation** classifies tens of thousands of graphs at once at
  **~90 million vertices / second** — about **190×** a naive per-graph loop.

## A quick taste

Bipartiteness as a monadic second-order formula, compiled **once** for the whole
class of tree-depth-≤3 graphs into a 6-state automaton, then decided on any graph
in a single linear pass:

```python
import networkx as nx
from autstr.graphs import TreeDepthClass, TreeDepthGraph

cls = TreeDepthClass(3)
bipartite, _ = cls.evaluate(
    'exists c.(all x.(all y.((not E(x,y)) or '
    '((Subset(x,c) and (not Subset(y,c))) or '
    '((not Subset(x,c)) and Subset(y,c))))))')

triangle = TreeDepthGraph.from_networkx(nx.cycle_graph(3))
bipartite.accepts([(s,) for s in cls.advice(triangle)])   # False — in microseconds
```

## An experiment in AI-assisted algorithm engineering

Each major AutStr release doubles as a snapshot of what a frontier AI coding system
can do on hard, verifiable algorithmic work, with the mathematical direction and
review kept firmly human. Version 2.0 came out of an intensive two-day
pair-programming session inside Claude Code with Anthropic's Fable 5 model: it
profiled and rewrote the automata core, migrated the JAX dependency, and built the
entire uniformly-automatic layer — each class verified against exhaustive or exact
ground-truth oracles. Several constructions the author had sketched a decade earlier
went from a whiteboard description to running, tested code within hours. *The code
is the model's; the theory, the choices, and the verification protocol were human.*

## Who is it for?

AutStr is built for researchers and practitioners in algorithmic model theory,
logic, and theoretical computer science — anyone exploring computable infinite
structures or looking to turn declarative specifications into optimal streaming
algorithms.

Install with `pip install autstr` and see the [project page](/project/autstr/) and
the [showcase notebooks](https://github.com/fariedabuzaid/AutStr/tree/main/notebooks)
to get started.
