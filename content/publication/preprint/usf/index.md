---
hugoblox:
  ids:
    arxiv: 2510.09452
title: "On Uniformly Scaling Flows: A Density-Aligned Approach to Deep One-Class Classification"
authors:
  - Faried Abu Zaid
  - Tim Katzke
  - Emmanuel Neuer
  - Daniel Neider
date: "2025-09-29"
publishDate: "2025-10-13"
publication_types: ["article"]
publication: "CoRR"
publication_short: "arXiv"
volume: "abs:2510.09452"
abstract: |
  Unsupervised anomaly detection is often framed around two widely studied paradigms. Deep one-class classification, exemplified by Deep SVDD, learns compact latent representations of normality, while density estimators realized by normalizing flows directly model the likelihood of nominal data. In this work, we show that uniformly scaling flows (USFs), normalizing flows with a constant Jacobian determinant, precisely connect these approaches. Specifically, we prove how training a USF via maximum-likelihood reduces to a Deep SVDD objective with a unique regularization that inherently prevents representational collapse. This theoretical bridge implies that USFs inherit both the density faithfulness of flows and the distance-based reasoning of one-class methods. We further demonstrate that USFs induce a tighter alignment between negative log-likelihood and latent norm than either Deep SVDD or non-USFs, and how recent hybrid approaches combining one-class objectives with VAEs can be naturally extended to USFs. Consequently, we advocate using USFs as a drop-in replacement for non-USFs in modern anomaly detection architectures. Empirically, this substitution yields consistent performance gains and substantially improved training stability across multiple benchmarks and model backbones for both image-level and pixel-level detection. These results unify two major anomaly detection paradigms, advancing both theoretical understanding and practical performance. 
summary: |
  Uniformly scaling flows for anomaly detection unify density faithfulness with distance-based reasoning.
tags:
  - anomaly detection
  - normalizing flows
  - probabilistic modeling
  - machine learning
featured: true
links:
  - type: doi
    url: https://arxiv.org/abs/2510.09452
image:
  caption: 'Image credit: Tim Katzke'
  focal_point: ""
  preview_only: false
projects: []
slides: ""
---

