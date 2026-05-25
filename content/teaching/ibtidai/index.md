---
title: "Ibtidai — AI-Driven Learning Studio"
authors:
  - Faried Abu Zaid
date: "2026-05-25"
links:
  - type: site
    url: https://ibtidai.netlify.app
featured: true
image:
  caption: 'Image credit: ChatGPT'
  focal_point: ""
  preview_only: false
---
ibtidai is an open-access AI learning platform designed for self-directed, long-term skill development. It is a practical experiment in applying large language models and adaptive learning techniques to the problem of structured, personalized education outside of formal institutional settings.

## Concept

The platform operationalizes several ideas from the learning sciences:

- **Curriculum decomposition** — a learner-defined goal is automatically broken into a coherent sequence of focused lecture units
- **Adaptive path selection** — the next topic is chosen based on mastery estimates, error rates, and learner preference signals
- **Spaced repetition** — targeted question sessions reinforce weak concepts at algorithmically determined intervals
- **Concept-level mastery tracking** — each concept carries a mastery score updated using a blended model of recent performance and historical accuracy

## Content Generation

Lectures are generated on-demand using the learner's preferred LLM provider (OpenAI, Anthropic Claude, Google Gemini, or Groq). Web grounding is optionally enabled to anchor content in current sources; reference links are included in each lecture.

Learners supply their own API key, retaining full control over provider choice and cost. Efficient models such as Claude Haiku keep typical usage costs to cents per day.

## Access

The platform is currently in early access at **https://ibtidai.netlify.app** and open to anyone interested in trying adaptive, AI-driven self-study.

