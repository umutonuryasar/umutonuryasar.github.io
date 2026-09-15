---
title: "MicroCLIP — Contrastive Vision-Language Learning on a Single GPU"
excerpt: "A from-scratch CLIP-style vision-language project studying sigmoid vs. softmax contrastive objectives and batch size under a constrained single-GPU budget."
collection: portfolio
---

## Overview

**MicroCLIP** is a small CLIP-style dual-encoder vision-language model built to study contrastive learning under realistic compute constraints rather than chase leaderboard-scale results.

The model pairs a **ResNet-18 image encoder** with a **4-layer Transformer text encoder implemented from scratch** and trains on COCO Captions. The project is structured as a compute-normalized experimental study: the central question is how much the contrastive objective — SigLIP-style sigmoid loss versus softmax InfoNCE — matters relative to batch size when training resources are limited.

## Research Question

> Under small-batch, single-GPU constraints, how much does the choice of contrastive loss matter relative to batch size?

The experiment matrix controls loss and batch size while also providing secondary ablations for optimizer, learning-rate schedule, initialization, and image encoder architecture.

## Evaluation

The evaluation pipeline covers:

- **Image–text retrieval** on COCO 5K and Flickr30k 1K
- **Zero-shot classification** on CIFAR-10 and CIFAR-100
- **Controlled ablations** across sigmoid/softmax objectives and batch sizes
- **Weights & Biases logging** for experiment tracking

The GitHub version of the project currently documents the implementation and evaluation setup. Training work is ongoing, so this page intentionally avoids presenting preliminary runs as final conclusions.

## Tech Stack

`PyTorch` · `ResNet-18` · `Transformer` · `Contrastive Learning` · `COCO Captions` · `Weights & Biases`

## Links

- **Repository:** [github.com/umutonuryasar/microclip](https://github.com/umutonuryasar/microclip)
- **Project proposal:** [docs/PROPOSAL.md](https://github.com/umutonuryasar/microclip/blob/main/docs/PROPOSAL.md)
