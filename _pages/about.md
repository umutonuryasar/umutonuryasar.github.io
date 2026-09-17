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
    <h1>Efficient AI, tested under real constraints.</h1>
    <p class="home-intro">I build and study computer vision and multimodal systems, with a focus on controlled experiments, model efficiency, and reproducible engineering.</p>
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
        <p>A from-scratch CLIP-style study testing whether SigLIP's reported small-batch advantage over softmax persists at small scale; in this regime, softmax matched or outperformed sigmoid across the tested batches.</p>
        <div class="work-tags"><span>PyTorch</span><span>CLIP</span><span>Vision-Language</span></div>
      </article>

      <article class="work-item">
        <div class="work-meta"><span>02</span><span>Computer Vision · Technical Report</span></div>
        <h2><a href="/rt-detr-kd/">RT-DETR Knowledge Distillation <span aria-hidden="true">→</span></a></h2>
        <p>An 11-configuration controlled study across five KD methods for RT-DETR on a 4 GB GPU, where explicit controls changed the interpretation of two transformer-specific ideas.</p>
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
      <p class="home-section-note">Independent research and publications.</p>
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
      <a class="oss-item" href="https://github.com/huggingface/peft/pull/3293"><span><strong>Hugging Face PEFT</strong><small>Memory-cache fix for k-bit training, with XPU support</small></span><span class="oss-status">PR #3293 · Merged ↗</span></a>
      <a class="oss-item" href="https://github.com/andrewyng/aisuite/pull/319"><span><strong>aisuite</strong><small>Python 3.14 compatibility via dependency constraint fix</small></span><span class="oss-status">PR #319 · Merged ↗</span></a>
    </div>
  </section>

  <section class="home-section home-about" id="about">
    <div class="home-section-heading"><p class="home-kicker">About</p></div>
    <div class="about-grid">
      <p>My background is in Electrical &amp; Electronics Engineering; my current work is independent applied-AI research spanning efficient learning, computer vision, and multimodal models. I work from research question to controlled experiment to implementation, with an emphasis on reproducibility and practical constraints.</p>
      <p class="about-secondary">Based in Ankara, Türkiye. Open to research engineering roles, collaborations, and technically ambitious applied AI work.</p>
    </div>
    <div class="home-contact">
      <a href="https://github.com/umutonuryasar">GitHub ↗</a>
      <a href="https://www.linkedin.com/in/umutonuryasar">LinkedIn ↗</a>
      <a href="mailto:umutonuryasar@gmail.com">Email ↗</a>
    </div>
  </section>
</div>
