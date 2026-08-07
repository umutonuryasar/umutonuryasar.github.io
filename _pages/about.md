---
permalink: /
title: "About Me"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I am an Applied AI Research Engineer working at the intersection of **Deep Learning** and **Computer Vision**, focused on building efficient, scalable AI systems across the full research-to-deployment stack. My work spans knowledge distillation, object detection, and model compression, with an emphasis on production-grade applied research.

## Research Interests

- **Knowledge Distillation & Parameter-Efficient Fine-Tuning** — teacher-student training, adapters/PEFT, model compression
- **Computer Vision** — object detection, visual representation learning, DETR-family architectures
- **Applied AI Research** — efficient, low-VRAM training, real-time detection, and reproducible evaluation

## Selected Projects

**[detrflow — End-to-End RT-DETR Object Detection Pipeline](https://github.com/umutonuryasar/detrflow)**
Production-ready object detection pipeline built on RT-DETR, reproducing the pretrained baseline (**AP = 53.1** on COCO val2017). Includes a FastAPI inference API, HuggingFace Space deployment, and benchmark scripts. A systematic knowledge distillation study on RT-DETR was run as a companion project — see [the tech report](/rt-detr-kd/).

**[Distilling RT-DETR on a 4 GB GPU](/rt-detr-kd/)**
A controlled knowledge-distillation study on RT-DETR — five KD methods tested against control runs, three of them my own, all three refuted by their own controls. Full tech report and a three-post write-up series covering the methods, the query-matching control, and what the controls revealed about λ scheduling.

**[Student Capacity Moderates Knowledge Distillation Effectiveness](https://github.com/umutonuryasar/kd-capacity-gap)**
Systematic study of Logit-KD and Feature-KD across three ResNet teacher-student pairs on CIFAR-10. Key finding: student capacity — not the teacher-student accuracy gap — is the primary moderating factor in KD effectiveness. Results reproduced across 3 seeds; interactive demo on HuggingFace Spaces. Published as an arXiv preprint ([arXiv:2605.31191](https://arxiv.org/abs/2605.31191)).

## Open Source Contributions

**[HuggingFace PEFT — PR #3293](https://github.com/huggingface/peft/pull/3293)**
Fixed a CUDA memory caching bug in the PEFT library; reviewed and merged into the main branch.

## Background

I hold a degree in Electrical and Electronics Engineering, which underpins the applied research above — an arXiv preprint, a merged PEFT contribution, and an RT-DETR tech report. I also maintain an active [blog](/year-archive/) where I write about deep learning, research papers, and applied AI.

## Get in Touch

Open to research discussions, collaborations, or exchanging ideas. You can reach me via [GitHub](https://github.com/umutonuryasar) or [LinkedIn](https://www.linkedin.com/in/umutonuryasar).