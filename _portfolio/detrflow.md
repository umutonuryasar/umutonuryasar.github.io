---
title: "detrflow – End-to-End RT-DETR Object Detection Pipeline"
excerpt: "A production-ready object detection pipeline built on RT-DETR, reproducing the pretrained baseline (AP=47.9 on COCO val2017). Features a deployed HuggingFace Space, FastAPI inference API, and benchmark tooling.<br/><img src='/images/detrflow-preview.jpg' style='max-width: 360px; width: 100%;'>"
collection: portfolio
---

## Overview

**detrflow** is a fully end-to-end object detection pipeline built on [RT-DETR](https://arxiv.org/abs/2304.08069) (Real-Time Detection Transformer), covering everything from data loading and training to evaluation, serving, and deployment.

The project demonstrates a complete applied ML workflow — from pretrained baseline evaluation to a live inference API. A systematic knowledge distillation study on RT-DETR is in progress as a companion project.

## Key Results

| Metric                     | Value                       |
| -------------------------- | --------------------------- |
| COCO val2017 AP (baseline) | **47.9**                    |
| Backbone                   | RT-DETR (ResNet-50)         |
| Evaluation dataset         | COCO val2017 (5,000 images) |

## Features

- **COCO val2017 Evaluation** — Full benchmark against the standard detection suite with reproducible results
- **FastAPI Inference API** — REST endpoint for single-image and batch inference
- **HuggingFace Space Deployment** — Live demo hosted and publicly accessible
- **Benchmark Scripts** — Latency and throughput profiling utilities
- **Model Card** — Documented limitations, intended use, and evaluation results

## Tech Stack

`PyTorch` · `RT-DETR` · `HuggingFace Transformers` · `FastAPI` · `COCO API` · `Docker`

## Links

- 💻 **GitHub Repository:** [github.com/umutonuryasar/detrflow](https://github.com/umutonuryasar/detrflow)
- 🤗 **HuggingFace Space:** [huggingface.co/spaces/umutonuryasar/detrflow](https://huggingface.co/spaces/umutonuryasar/detrflow)

## Related Work

This project builds on the RT-DETR architecture and serves as the empirical foundation for an ongoing knowledge distillation study on RT-DETR. A related, completed study on KD dynamics across ResNet teacher-student pairs is available as an arXiv preprint ([arXiv:2605.31191](https://arxiv.org/abs/2605.31191)).