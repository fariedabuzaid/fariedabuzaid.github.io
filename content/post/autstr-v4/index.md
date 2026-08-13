---
title: "AutStr 4.0 — A Fruitful Summer: Trees, Rank-Width, and Decidable Reachability"
summary: Three major releases since 2.0 in a single summer — tree-automatic structures and MTBDD transitions in 3.0, bounded rank-width families and implicit evaluation in 3.1, and now symbolic queries, computed interpretations, and reachability in collapsible pushdown graphs.
date: 2026-08-14T00:40:26+02:00

authors:
  - admin

tags:
  - Academic
  - Project
  - Release
  - Automatic Structures
  - Model Theory

image:
  caption: 'The integers greater than 1 divisible by neither 2 nor 3 — an infinite set as a nine-state automaton, computed by AutStr'
  focal_point: ""
  preview_only: false

content_meta:
  trending: true

math: true
---

<style>
/* The featured automaton is a line drawing: black ink on white. Invert it when
   the site is in dark mode so it reads as white ink on near-black, instead of a
   bright card in a dark page. Scoped to this post -- the rule ships inside it. */
html.dark .featured-image-wrapper img { filter: invert(1) hue-rotate(180deg); }
</style>

This has been an unusually fruitful summer for AutStr. Four major releases
landed between early July and mid-August, each building on the one before:

| | | |
|---|---|---|
| **2.0** | 7 July | uniformly automatic classes; a 10²–10³× faster batched-NumPy core |
| **3.0** | 10 July | from words to **trees**; transitions become shared decision diagrams; composition |
| **3.1** | 19 July | bounded **rank-width** groups and graphs; the chain-ring extension; implicit evaluation |
| **4.0** | 13 August | **symbolic queries**; computed interpretations; infinite graphs and decidable reachability |

Only 2.0 got [an announcement of its own](/post/autstr-v2/), so this post covers
everything since — the two releases in between, and then 4.0 in full.

`pip install autstr` · GitHub: [fariedabuzaid/AutStr](https://github.com/fariedabuzaid/AutStr)
· [Documentation](https://fariedabuzaid.github.io/AutStr/)

## 3.0 — from words to trees

Version 2 presented infinite structures as automata reading *words*. Version 3
added the tree counterpart of the entire stack: bottom-up tree automata,
tree-automatic presentations, and uniformly tree-automatic classes. That is the
step from Büchi's theorem to Rabin's, and it buys structures that no word
automaton can present — **Skolem arithmetic** (ℕ, ·), where a number is the tree
of its prime exponents, and graphs of bounded **tree-width** and
**clique-width** with full MSO. It was cross-validated by embedding the word
engine's Büchi arithmetic into the tree engine and re-deciding every sentence
through both.

Underneath both engines, a transition stopped being a `symbol → target` table
and became a **shared multi-terminal decision diagram** over the symbol's
digits — the representation MONA uses, for MONA's reason: over a convolution
alphabet the flat table is the bottleneck. Queries that had been impossible for
lack of alphabet width suddenly compiled. An arity-5 relation over a 14-letter
alphabet — 14⁵ = 537 824 flat symbols — went from *infeasible* to 0.2 s, and
tree-depth-4 bipartiteness from 17 s to 0.4 s.

Version 3.0 also added `autstr.composition`: disjoint unions and direct products
of structures, and unions and product closures of whole classes. Composed, they
present every finite direct product of index-≤2 cyclic and extraspecial
*p*-groups — and decide that such a product is abelian exactly when all of its
factors are.

## 3.1 — rank-width, and queries that build no automaton

Where tree-width bounds how much *combinatorial* structure crosses a cut,
**rank-width** bounds the linear-algebraic rank of what crosses. Version 3.1
presented both graphs and groups by that measure from one body of machinery:
`RankWidthClass` for graphs, and class-2 groups of bounded rank-width whose
advice spells out rank-≤r factorizations of the commutation form's crossing
blocks, cut by cut. The **chain-ring extension** then generalised all of it from
F_p to ℤ/p^d — Smith normal form, saturated interfaces, widths measured as
module cut-rank — and is byte-identical at d = 1.

The other half of 3.1 was **implicit evaluation**. Some members of these classes
have automata far too large to build. `check_implicit` and `evaluate_implicit`
decide first-order formulas and compute satisfying sets *without ever building a
query automaton* — or even a base automaton — reaching members that are
otherwise entirely out of reach.

## 4.0 — the symbolic release

Which brings us to today. Where 2.0 made the engine fast and 3.0 took it to
trees, version 4 is about how you *talk* to it — and about two structures that
sit right at the edge of what first-order logic can decide.

### Write the mathematics, not the formula string

Queries used to be strings. Now every structure hands out variables that compose
with ordinary Python operators, and every structure declares its own operators
*and its own codec* — so the values you put in are the values you get back:

```python
from autstr.arithmetic import BuechiArithmeticZ

Z = BuechiArithmeticZ().symbolic()      # (ℤ, +, <, |₂); no setup required
x, y, z = Z.vars('x y z')

R = (x + y + 3).lt(2 * x)     # the infinite set { (x, y) : x + y + 3 < 2x }
R.is_empty()                  # False
(0, 4) in R                   # False   — membership test

band = (x + y).eq(z) & z.gt(0) & z.lt(3)   # x + y = z ∧ 0 < z < 3
for s, _ in zip(band.drop(z), range(3)):   # ∃z, then enumerate smallest-first
    print(s)                               # (0, 1), (1, 0), (1, 1)
```

Nothing is materialised until you iterate; `& | ~` are exact operations on
infinite sets. The same expressions work over any structure or class in the
library, on either the word or the tree engine. Over the finite-powerset
structure, for instance, the Boolean operations *are* the arithmetic ones:

```python
from autstr.powerset import MSO0

a, b, c = MSO0().symbolic().vars('a b c')   # finite sets of naturals
({0, 1}, {1, 2}, {0, 1, 2}) in (a + b).eq(c)       # union — True
```

By Büchi's theorem, first-order logic over that structure *is* monadic
second-order logic over (ℕ, <) — so quantifying over sets there costs no more
than quantifying over elements does elsewhere.

### Structures defined inside other structures

Automatic structures are closed under first-order interpretation. AutStr 4.0
does not merely record that fact — `interpret` **computes** the interpreted
presentation from a domain formula, a formula per relation, a dimension, and
optionally a definable equivalence whose classes become the elements.

The textbook example is the construction of the integers from the naturals: a
pair *(a, b)* stands for *a − b*, and two pairs denote the same integer when
*a + d = c + b*. That is a two-dimensional quotient interpretation, and it runs.

Quotients need one representative per class. Over words that is the
shortlex-least member. Over **trees** no automatic order is well-founded —
growing a tree where two members differ makes it *smaller* — so a class may have
no least member at all, and the representative is instead the least
*description* (Kuske & Weidner). Both work.

The ordinals are the payoff: by Cantor normal form an ordinal below ω<sup>n</sup>
is an *n*-tuple of naturals in reverse-lexicographic order, so `autstr.ordinals`
is three formulas over Büchi arithmetic with no automaton written by hand.

```python
from autstr.ordinals import Ordinal

p, q = Ordinal(2).symbolic().vars('p q')   # the ordinals below ω²
((0, 5), (1, 0)) in p.lt(q)                # 5 < ω  — True
```

How far that reaches is exactly Delhommé's theorem: the word-automatic ordinals
are those below ω<sup>ω</sup>, the tree-automatic ones those below
ω<sup>ω<sup>ω</sup></sup>.

### Where first-order logic stops — and where it doesn't

This is my favourite part of the release, because the two new structures make a
sharp contrast.

A **Turing machine's configuration graph** is automatic: a configuration is a
state, a tape and a head position, one step rewrites a bounded window, and the
whole first-order theory is decidable. What you may *not* ask is whether one
configuration **reaches** another — that is the halting problem, and no
first-order formula over this graph can express it.

**Level 2 collapsible pushdown graphs** are where that changes. Their stacks are
stacks of stacks whose letters carry collapse links, and their configuration
graphs are tree-automatic by Kartzow's encoding — blocks hang off each other as
a tree, and the links are recovered from its shape rather than stored. Since MSO
over these graphs is undecidable, the tree encoding is the *only* automatic
route to them.

And there, reachability is a relation of the graph like any other:

```python
from autstr.collapsible import Level2CPS

system = Level2CPS([('0', None, 'c', '1', 'clone'),
                    ('1', None, 'o', '0', 'pop 2')], symbols=('a',))
graph = system.configuration_graph()

graph.presentation.relation('Reach')       # 29 states, built in 0.5 s
graph.check('all x.(all y.(all z.((Reach(x,y) & Reach(y,z)) -> Reach(x,z))))')
# True — transitivity, over all configurations and all runs of any length
```

That last line is worth pausing on. It is a statement about *every* run between
*every* pair of configurations, decided by automata operations in under a
second — and it is not a test that passed, it is a theorem that was proved.

Following Kartzow, reachability is not a fixpoint over trees but a decomposition:
every run splits into four stretches — words leaving the stack, letters leaving,
letters returning, words returning — each relation reflexive, so

$$\mathrm{Reach}(x,y) \;=\; \exists d\, \exists f\, \exists g.\; A(x,d) \wedge B(d,f) \wedge C(f,g) \wedge D(g,y)$$

is a first-order formula over four automata. It is exponential in the number of
control states, so it is declared up front and built the first time a query
mentions it.

### Also new

- **The countable atomless Boolean algebra** — the clopen algebra of Cantor
  space, where every nonzero element splits forever, on the tree engine.
- **Infinite graphs** — the Cayley graph of ℤⁿ and the infinite *k*-regular tree
  with its prefix order.
- **Serialization for the tree engine**, so an expensive relation is built once
  and kept.

## Breaking changes

`autstr.buildin` is gone: every structure the library ships is built in, so the
name carved nothing, and its contents moved to modules named for their subject —
`autstr.arithmetic`, `autstr.powerset`, `autstr.tree_arithmetic`. The
[changelog](https://github.com/fariedabuzaid/AutStr/blob/main/CHANGELOG.md)
has the upgrade path.

One fix worth calling out for 3.x users: a sentence used as an operand of a
connective — `(all x.(...)) & (exists y.(...))` — used to raise `IndexError`. It
works now, on both engines.

## An experiment in AI-assisted algorithm engineering

Each major AutStr release doubles as a snapshot of what a frontier AI coding
system can do on hard, verifiable algorithmic work, with the mathematical
direction and review kept firmly human. That is what made this summer possible:
four major releases in five weeks, each a focused session in Claude Code — the
tree engine and the decision-diagram rewrite in 3.0, the rank-width families and
the chain-ring generalisation in 3.1, and in 4.0 the symbolic layer, the
interpretation machinery, Kartzow's reachability construction worked through
from the paper, and a serializer for the tree engine.

Several of the constructions realised over these weeks were sketched on a
whiteboard a decade ago and never implemented; some went from that sketch to
running, tested code within hours.

The part I found most instructive was not the new code but what writing the
*documentation* uncovered. Building the worked examples for the notebooks turned
up three genuine defects that the 850-test suite had never touched — including
enumeration being silently broken for every interpreted structure of dimension
greater than one, in a released version, because membership queries happened to
take a different code path. A good example exercises a library in ways a test
suite does not.

*The code is the model's; the theory, the choices, and the verification protocol
are human.*

## Who is it for?

AutStr is built for researchers and practitioners in algorithmic model theory,
logic, and theoretical computer science — anyone exploring computable infinite
structures, or turning declarative specifications into optimal streaming
algorithms.

Install with `pip install autstr` and see the [project page](/project/autstr/),
the [documentation](https://fariedabuzaid.github.io/AutStr/), and the
[showcase notebooks](https://github.com/fariedabuzaid/AutStr/tree/main/notebooks)
to get started.
