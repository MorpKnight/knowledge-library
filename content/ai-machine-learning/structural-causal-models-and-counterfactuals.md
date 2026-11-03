---
title: Structural Causal Models and Image Counterfactuals
description: What causal graphs and counterfactual generation add beyond a VAE.
date: 2026-11-04
tags: [causal-inference, counterfactuals, generative-models]
publish: true
draft: false
---

## The question

Can a generative model help reason about what would have happened to the same person under a different condition? A plausible altered image alone is not enough. This note synthesizes literature study; it does not report a clinical counterfactual experiment.

## Three different questions

1. **Association:** how does `P(Y | X=x)` vary among observed cases?
2. **Intervention:** what would the distribution of `Y` be if a defined mechanism for `X` were replaced by `do(X=x)`?
3. **Counterfactual:** for a particular observed unit, what would `Y` have been under an alternative intervention?

An SCM specifies endogenous variables, structural assignments, exogenous variation, and a causal graph. Parent choices and assumptions about unmeasured factors carry substantive meaning. A probabilistic fit or a learned latent representation cannot validate those assumptions on its own.

## Abduction, action, prediction

Given an observed unit, **abduction** infers the exogenous variation compatible with its observations; **action** replaces an equation with the chosen intervention; **prediction** propagates that intervention through the remaining mechanisms. The procedure is meaningful only to the extent that the assumed graph, measured variables, intervention, and inferred exogenous state are adequate. Unmeasured confounding and non-identifiability may prevent a unique answer.

Deep SCMs can model complex mechanisms using neural networks, flows, and VAEs. The Pawlowski paper explores tractable counterfactual inference, including image examples. Ribeiro and colleagues study high-fidelity image counterfactuals, hierarchical latent models, mediation, and evaluation. Preserving apparent identity and producing sharper images are relevant, but neither alone validates a clinical counterfactual. Assess consistency and effectiveness with the model's assumptions, uncertainty, image fidelity, and domain review.

## Relation to reinforcement learning

Reinforcement learning studies sequences of actions, rewards, policies, and feedback in an environment. Causal RL adds a causal framing to issues such as confounding and distribution shift. It is relevant when a real sequential decision problem, actions, outcomes, and feedback have been defined; it is not automatically a method for identifying the cause of TB from observational images.

## Practical questions for medical imaging

Define the population, unit, intervention, time ordering, outcome, confounders, selection, and measurement process before selecting the generator. Validate graph assumptions with domain experts. Distinguish an algorithm score from a clinical diagnosis and regional environmental proxies from individual exposure. Identify plausible interventions and seek evidence on the original data's labels, missingness, and shifts. These are research questions, not validated clinical claims.

## Sources

- Pawlowski et al., [Deep Structural Causal Models](https://papers.nips.cc/paper/2020/hash/0987b8b338d6c90bbedd8631bc499221-Abstract.html).
- De Sousa Ribeiro et al., [High Fidelity Image Counterfactuals with Probabilistic Causal Models](https://proceedings.mlr.press/v202/de-sousa-ribeiro23a.html).
- Sutton and Barto, [Reinforcement Learning: An Introduction](https://mitpress.mit.edu/9780262039246/reinforcement-learning/). The workspace note is a foundations map, not a complete reading of the book.
- Local evidence: `tpu-project/learning/04-pawlowski-dscm.md`, `05-ribeiro-high-fidelity-counterfactuals.md`, `06-sutton-barto-reinforcement-learning.md`, and `06b-deng-causal-reinforcement-learning.md`.
- Related: [[ai-machine-learning/variational-inference-vae-and-graphical-models|Variational inference and VAEs]].
