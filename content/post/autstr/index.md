---
title: AutStr 1.0.1
summary: Manipulate Infinite Data Structures in Python
date: 2023-07-17

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'

authors:
  - admin

tags:
  - Academic
  - Project
  - Release

content_meta:
  trending: true
---

I'm excited to announce the release of AutStr, a Python library for symbolic manipulation of infinite mathematical structures using automata theory.

## What is AutStr?
AutStr brings the power of automatic structures research to Python, enabling:

**🔢 Infinite Arithmetic**
- Native algebraic interface for Büchi integer arithmetic
- Solve linear equations over ℤ with base-2 encoding
- Symbolic implementation of algorithms like the Sieve of Eratosthenes

**🤖 Automatic Presentations**
- Represent infinite domains and relations via finite automata
- Evaluate FO(∞) queries ("there exist infinitely many x")
- Dynamically update relations during evaluation
- Serialize compiled results for reuse

**⚡️ Sparse Automata Backend**
- JAX-accelerated automata operations
- Space-efficient representation via default transitions
- Integrated visualization capabilities

### Quick Example
```python
from autstr.arithmetic import x, y
R = (x + y == 10)  # Infinite solution set
print(R.is_finite())  # False
print((5, 5) in R)   # True
```

GitHub: [fariedabuzaid/AutStr](https://github.com/fariedabuzaid/AutStr)
Install: `pip install autstr`

## Who is it for?
AutStr is perfect for researchers and anyone exploring computable infinite structures, especially in model theory, logic, and theoretical computer science.

## How does AutStr compare?
| Feature         | AutStr                  | SymPy/Z3                |
|-----------------|------------------------|-------------------------|
| Domain          | Infinite structures     | Finite/closed-form math |
| Representation  | Automata-based         | Symbolic expressions    |
| Quantifiers     | Native ∞-quantifiers   | ∀/∃ with limitations    |
| Solutions       | Entire solution sets    | Single solutions        |
| Specialty       | Decidable infinite models | General math/SAT     |

**When to use AutStr:**
- For infinite domains (ℤ, ℚ, trees)
- When you need complete solution sets
- For FO(∞) queries

**Not for:**
- Logic fragments where highly optimized solvers exist

SymPy/Z3 excel at symbolic algebra and SAT, while AutStr specializes in automata-representable infinities and first-order logic with specialized quantifiers. They complement, not replace, each other.

