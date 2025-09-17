---
title: USFlows
date: 2025-07-01
links:
  - type: code
    url: http://github.com/aai-institute/USFlows
title: "USFlows: Flow models with density preserving latent space alignment"
authors:
  - Faried Abu Zaid
  - appliedAI Institute for Europe 
  - Contributors
featured: true
image:
  caption: 'Image credit: ChatGPT'
  focal_point: ""
  preview_only: false
tags:
  - Normalizing Flows
  - Anomaly Detction
  - Neuro-Symbolic Verification
---

USFlows provides a rigorous implementation of flow models with a constant Jacobian determinant, ensuring uniformly scaling transformations. The framework offers robust theoretical guarantees for latent space alignment: density level sets in the data space are mapped exactly to corresponding level sets in the latent space. This property enables precise level set definitions, which are essential for advanced applications such as anomaly detection and neuro-symbolic verification.
In addition, USFlows includes a comprehensive suite of base distributions, notably radial distributions with learnable norm parameters for various norms. This flexibility allows for highly accurate and adaptable level set alignment across diverse modeling scenarios.

## References
  - Abu Zaid, F., Neider, D., Yalciner, M. VeriFlow: Modeling Distributions for Neural Network Verification, 2024. [DOI](https://arxiv.org/abs/2406.14265)