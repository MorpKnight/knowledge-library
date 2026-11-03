---
title: From Probabilistic Graphs to Variational Autoencoders
description: How graphical models, variational inference, and amortized inference fit together.
date: 2026-11-04
tags: [probability, variational-inference, vae]
publish: true
draft: false
---

## Question and scope

How can we reason about complicated distributions when the posterior is expensive to compute, and how does that lead to a variational autoencoder (VAE)? This is a synthesis of the workspace's literature notes, not a new model experiment. The source notes were prepared in 2026; no clinical experiment follows from them.

## Graphical models

A probabilistic graphical model represents a joint distribution using a graph. A directed Bayesian network factors the joint into conditional distributions of each node given its parents; an undirected Markov network instead uses compatible factors. The graph provides a language for conditional independence, compact representation, inference, and learning. A probabilistic edge does not automatically establish a causal mechanism.

Inference asks for an unobserved quantity given evidence. Computing the exact posterior can require integrating over a high-dimensional latent space. Variational inference turns the problem into an optimization: choose an approximating family `q(z)` and minimize its divergence from the true posterior. The family and objective make calculation tractable, but also restrict what can be represented.

## VAE construction

For observed `x` and latent `z`, the generative model factors as `pθ(x,z) = pθ(z) pθ(x|z)`. An encoder `qφ(z|x)` approximates the generally intractable posterior. The evidence lower bound is

`ELBO = E_q[log pθ(x|z)] − KL(qφ(z|x) || pθ(z))`.

The first term rewards explaining the observation; the second regularizes the approximate posterior toward the prior. Maximizing the ELBO optimizes a lower bound, not the exact likelihood in general. An encoder amortizes inference across examples rather than independently fitting a posterior for each input.

For a diagonal Gaussian encoder, sample `ε ~ N(0,I)` and compute `z = μφ(x) + σφ(x) ⊙ ε`. The randomness is separated from the differentiable transformation, enabling minibatch stochastic-gradient training of encoder and decoder. A training step encodes a minibatch, draws noise, decodes the resulting latent samples, evaluates the ELBO estimate, and updates the parameters.

## What this does and does not establish

The original AEVB paper reports experiments on MNIST and Frey Face; those are the authors' results, not a reproduction performed for this note. Approximate posteriors can be inaccurate, and a useful reconstruction or interpretable latent direction does not identify a cause. In chest X-rays, a visually plausible reconstruction might omit clinically relevant small findings. Evaluate reconstruction, downstream behavior, dataset shift, and clinical relevance separately. See [[ai-machine-learning/structural-causal-models-and-counterfactuals|SCMs and counterfactuals]] for the additional assumptions needed for a causal question.

## Sources and reading limits

- Jordan et al., [An Introduction to Variational Methods for Graphical Models](https://people.eecs.berkeley.edu/~jordan/papers/variational-intro.pdf).
- Kingma and Welling, [Auto-Encoding Variational Bayes](https://arxiv.org/abs/1312.6114). The local reading note used arXiv revision v11.
- Koller and Friedman, [Probabilistic Graphical Models](https://mitpress.mit.edu/9780262013192/probabilistic-graphical-models/): the workspace reading covered the publisher's sample first chapter, not the entire book.
- Workspace sources inspected: `tpu-project/learning/01-jordan-variational-inference.md`, `02-kingma-welling-vae.md`, and `03-koller-friedman-pgm.md`. These are local research materials, not public links.
