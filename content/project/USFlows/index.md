---
title: USFlows
date: 2025-07-01
title: "Flow models with density preserving latent space alignment"
authors:
  - Faried Abu Zaid
  - appliedAI Institute for Europe 
  - Contributors
repo_url: https://github.com/aai-institute/USFlows
featured: true
tags:
  - Normalizing Flows
  - Anomaly Detction
  - Neuro-Symbolic Verification
---

USFlows offers a robust implementation of flow models characterized by a constant Jacobian determinant, enabling uniformly scaling transformations. The framework provides strong theoretical guarantees regarding latent space alignment: density level sets in the data space are mapped precisely to corresponding level sets in the latent space. This property facilitates exact level set definitions, which are particularly valuable for applications such as anomaly detection and neuro-symbolic verification.

## References
  - Abu Zaid, F., Neider, D., Mustafa, Y. VeriFlow: Modeling Distributions for Neural Network Verification, 2024. [DOI](https://arxiv.org/abs/2406.14265)