---
title: AutStr
date: 2025-07-01
links:
  - type: code
    url: https://github.com/fariedabuzaid/AutStr
title: "AutStr: Symbolic Infinite Structures in Python"
authors:
  - Faried Abu Zaid
featured: true
tags:
  - Automatic Structures
  - Linear Integer Arithmetic
  - Symbolic Computation
  - First Order Logic
  - Monadic Second Order Logic
---

  AutStr is a Python library for symbolic representation and manipulation of infinite relational structures. It provides automata-based computation, first-order logic queries, and visualization tools for researchers and practitioners in algorithmic model theory.

  Key features include:
  - Symbolic infinite sets and relations
  - Automata-based computation
  - First-order logic interface
  - Visualization tools

  Example algorithms, such as the Sieve of Eratosthenes, demonstrate symbolic computation over infinite sets.

## Installation
  ```bash
  pip install autstr
  ```

## Example
  ### Sieve of Eratosthenes (Symbolic Infinite Set)
  ```python
  def infinite_sieve(steps):
      """Sieve of Eratosthenes over infinite integers"""
      candidates = (x.gt(1))  # Initial infinite set: {2,3,4,...}
      primes = []
      for _ in range(steps):
          for p in candidates:
              primes.append(p[0])
              break
          p = primes[-1]
          y = Var("y")
          multiples = (x.eq(p * y)).drop("y")
          candidates = candidates & ~multiples
      return primes, candidates
  # Execute first 3 sieving steps
  primes, remaining = infinite_sieve(steps=3)
  print(f"Primes found: {primes}")
  print(f"Remaining infinite set:")
  remaining.evaluate().show_diagram()
  ```

## References
  - Abu Zaid, F. Algorithmic Solutions via Model Theoretic Interpretations. Dissertation, RWTH Aachen University, 2016. [DOI](https://doi.org/10.18154/RWTH-2017-07663)
  - Blumensath, A., & Grädel, E. Automatic Structures. LICS 2000. [URL](https://lics.siglog.org/2000/Grdel-AutomaticStructures.html)
  - Khoussainov, B., & Nerode, A. Automatic presentations of structures. LCC 1994. [DOI](https://doi.org/10.1007/3-540-60178-3_93)
  - Khoussainov, B., Rubin, S., & Stephan, F. Automatic Structures: Richness and Limitations. LMCS 2007. [arXiv](https://arxiv.org/abs/cs/0703064) [DOI](https://doi.org/10.2168/LMCS-3%282%3A2%292007)
