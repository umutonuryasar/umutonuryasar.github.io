---
permalink: /
title: ""
author_profile: false
classes: home-redesign
redirect_from:
  - /about/
  - /about.html
---

<div class="home-shell">
  <section class="home-hero">
    <p class="home-eyebrow">Applied AI Research Engineer</p>
    <h1>Building and studying efficient learning systems.</h1>
    <p class="home-intro">I work across computer vision, multimodal learning, and model efficiency — turning research questions into reproducible experiments and practical systems.</p>
    <div class="home-actions">
      <a class="home-action home-action--primary" href="#work">View selected work</a>
      <a class="home-action" href="https://github.com/umutonuryasar">GitHub ↗</a>
    </div>
  </section>

  <section class="home-section" id="work">
    <div class="home-section-heading">
      <p class="home-kicker">Selected work</p>
      <p class="home-section-note">Research, experiments, and engineering.</p>
    </div>

    <div class="work-list">
      <article class="work-item">
        <div class="work-meta"><span>01</span><span>Multimodal Learning · Research</span></div>
        <h2><a href="/portfolio/microclip/">MicroCLIP <span aria-hidden="true">→</span></a></h2>
        <p>A from-scratch CLIP-style vision-language project for studying contrastive learning under tight compute constraints, with controlled experiments around training behavior and retrieval.</p>
        <div class="work-tags"><span>PyTorch</span><span>CLIP</span><span>Vision-Language</span></div>
      </article>

      <article class="work-item">
        <div class="work-meta"><span>02</span><span>Computer Vision · Technical Report</span></div>
        <h2><a href="/rt-detr-kd/">RT-DETR Knowledge Distillation <span aria-hidden="true">→</span></a></h2>
        <p>A controlled study of knowledge distillation for RT-DETR on a 4 GB GPU. Five strategies were tested against explicit controls, including negative results that changed the interpretation of the methods.</p>
        <div class="work-tags"><span>RT-DETR</span><span>Knowledge Distillation</span><span>Low-VRAM</span></div>
      </article>

      <article class="work-item">
        <div class="work-meta"><span>03</span><span>Computer Vision · Engineering</span></div>
        <h2><a href="/portfolio/detrflow/">DETRFlow <span aria-hidden="true">→</span></a></h2>
        <p>An end-to-end RT-DETR object detection pipeline reproducing the pretrained COCO baseline, with inference APIs, deployment, and benchmarking built around a reproducible workflow.</p>
        <div class="work-tags"><span>Object Detection</span><span>FastAPI</span><span>Deployment</span></div>
      </article>
    </div>
  </section>

  <section class="home-section" id="research">
    <div class="home-section-heading">
      <p class="home-kicker">Research</p>
      <p class="home-section-note">Published and ongoing work.</p>
    </div>
    <article class="research-feature">
      <div>
        <p class="research-year">2026 · arXiv preprint</p>
        <h2>Student Capacity Moderates Knowledge Distillation Effectiveness</h2>
        <p>A systematic study of Logit-KD and Feature-KD across ResNet teacher–student pairs, finding student capacity to be a stronger moderator of distillation effectiveness than the teacher–student accuracy gap.</p>
      </div>
      <div class="research-links">
        <a href="https://arxiv.org/abs/2605.31191">Paper ↗</a>
        <a href="https://github.com/umutonuryasar/kd-capacity-gap">Code ↗</a>
      </div>
    </article>
  </section>

  <section class="home-section" id="opensource">
    <div class="home-section-heading">
      <p class="home-kicker">Open source</p>
      <p class="home-section-note">Contributions upstream.</p>
    </div>
    <div class="oss-list">
      <a class="oss-item" href="https://github.com/huggingface/peft/pull/3293"><span><strong>Hugging Face PEFT</strong><small>CUDA memory caching fix</small></span><span class="oss-status">PR #3293 · Merged ↗</span></a>
      <a class="oss-item" href="https://github.com/andrewyng/aisuite/pull/319"><span><strong>aisuite</strong><small>Open-source contribution</small></span><span class="oss-status">PR #319 · Merged ↗</span></a>
    </div>
  </section>

  <section class="home-section home-about" id="about">
    <div class="home-section-heading"><p class="home-kicker">About</p></div>
    <div class="about-grid">
      <p>I am an Applied AI Research Engineer with an Electrical &amp; Electronics Engineering background. I am interested in efficient learning, computer vision, multimodal models, and the engineering required to turn experiments into reliable systems.</p>
      <p class="about-secondary">Based in Ankara, Türkiye. Open to research engineering roles, collaborations, and technically ambitious applied AI work.</p>
    </div>
    <div class="home-contact">
      <a href="https://github.com/umutonuryasar">GitHub ↗</a>
      <a href="https://www.linkedin.com/in/umutonuryasar">LinkedIn ↗</a>
      <a href="mailto:umutonuryasar@gmail.com">Email ↗</a>
    </div>
  </section>
</div>
