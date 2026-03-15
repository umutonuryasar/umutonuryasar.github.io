---
layout: archive
title: "About"
permalink: /about/
author_profile: true
---

<style>
.lang-toggle {
  display: flex;
  gap: 0;
  margin-bottom: 2rem;
  border: 1.5px solid #1a1a2e;
  border-radius: 6px;
  width: fit-content;
  overflow: hidden;
  font-family: 'Courier New', monospace;
  font-size: 0.78rem;
  letter-spacing: 0.08em;
}
.lang-btn {
  padding: 0.4rem 1.1rem;
  background: transparent;
  border: none;
  cursor: pointer;
  color: #888;
  font-family: inherit;
  font-size: inherit;
  letter-spacing: inherit;
  transition: all 0.18s;
}
.lang-btn.active {
  background: #1a1a2e;
  color: #fff;
}
.lang-content { display: none; }
.lang-content.visible { display: block; }

.about-section { margin-bottom: 2.2rem; }
.about-section h2 {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #999;
  margin-bottom: 0.7rem;
  font-weight: 500;
  border-bottom: 1px solid #eee;
  padding-bottom: 0.4rem;
}
.about-section p {
  font-size: 0.97rem;
  line-height: 1.75;
  color: #2a2a2a;
  margin: 0;
}
.stack-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 0.5rem;
  margin-top: 0.2rem;
}
.stack-item {
  font-family: 'Courier New', monospace;
  font-size: 0.82rem;
  color: #1a1a2e;
  background: #f4f6fb;
  border: 1px solid #dde3f0;
  border-radius: 4px;
  padding: 0.32rem 0.7rem;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}
.stack-dot {
  width: 5px; height: 5px;
  border-radius: 50%;
  background: #1E4FD8;
  flex-shrink: 0;
}
.focus-list {
  list-style: none;
  padding: 0; margin: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
}
.focus-list li {
  font-size: 0.83rem;
  color: #333;
  background: #fff;
  border: 1px solid #d0d8ee;
  border-radius: 4px;
  padding: 0.28rem 0.75rem;
  font-family: 'Courier New', monospace;
}
.open-to {
  font-size: 0.9rem;
  color: #1E4FD8;
  font-style: italic;
  margin-top: 1.4rem;
  border-left: 3px solid #1E4FD8;
  padding-left: 0.9rem;
  line-height: 1.65;
}
</style>

<div class="lang-toggle">
  <button class="lang-btn active" onclick="setLang('en')">EN</button>
  <button class="lang-btn" onclick="setLang('tr')">TR</button>
</div>

<!-- ─────────────────────────────── ENGLISH ─────────────────────────────── -->
<div id="content-en" class="lang-content visible">

  <div class="about-section">
    <h2>Background</h2>
    <p>
      AI/ML Engineer with a background in Electrical and Electronics Engineering,
      specializing in Deep Learning and Computer Vision. I work at the intersection
      of model efficiency and real-world deployment — designing, training, and
      optimizing neural networks with a focus on speed-accuracy trade-offs.
    </p>
  </div>

  <div class="about-section">
    <h2>Currently</h2>
    <p>
      Researching knowledge distillation applied to transformer-based object detectors
      (RT-DETR) as part of Stanford's CS229 curriculum — comparing Logit-KD and
      Feature-KD strategies across a 12-configuration ablation grid on COCO.
    </p>
  </div>

  <div class="about-section">
    <h2>Focus Areas</h2>
    <ul class="focus-list">
      <li>Object Detection</li>
      <li>Model Compression</li>
      <li>Knowledge Distillation</li>
      <li>Embedded AI</li>
      <li>RLHF</li>
      <li>CUDA Optimization</li>
    </ul>
  </div>

  <div class="about-section">
    <h2>Tech Stack</h2>
    <div class="stack-grid">
      <div class="stack-item"><span class="stack-dot"></span>PyTorch</div>
      <div class="stack-item"><span class="stack-dot"></span>TensorFlow</div>
      <div class="stack-item"><span class="stack-dot"></span>OpenCV</div>
      <div class="stack-item"><span class="stack-dot"></span>CUDA / C++</div>
      <div class="stack-item"><span class="stack-dot"></span>Python</div>
      <div class="stack-item"><span class="stack-dot"></span>MATLAB</div>
      <div class="stack-item"><span class="stack-dot"></span>Linux / Ubuntu</div>
      <div class="stack-item"><span class="stack-dot"></span>Git</div>
    </div>
  </div>

  <p class="open-to">
    Open to roles in Applied AI Research, Computer Vision Engineering, or MLOps.<br>
    Reach me at <a href="mailto:umutonuryasar@gmail.com">umutonuryasar@gmail.com</a>
  </p>

</div>

<!-- ─────────────────────────────── TURKISH ──────────────────────────────── -->
<div id="content-tr" class="lang-content">

  <div class="about-section">
    <h2>Hakkımda</h2>
    <p>
      Elektrik-Elektronik Mühendisliği altyapısıyla derin öğrenme ve bilgisayarlı görü
      alanında uzmanlaşmış bir AI/ML Mühendisiyim. Model verimliliği ve gerçek dünya
      dağıtımının kesişim noktasında çalışıyor; özellikle hız-doğruluk dengesi üzerine
      yoğunlaşıyorum.
    </p>
  </div>

  <div class="about-section">
    <h2>Güncel Çalışma</h2>
    <p>
      Stanford CS229 kapsamında transformer tabanlı nesne dedektörlerine (RT-DETR)
      uygulanan bilgi damıtma tekniklerini araştırıyorum — COCO veri seti üzerinde
      12 konfigürasyonluk bir ablasyon grid'i ile Logit-KD ve Feature-KD stratejilerini
      karşılaştırıyorum.
    </p>
  </div>

  <div class="about-section">
    <h2>Odak Alanları</h2>
    <ul class="focus-list">
      <li>Nesne Tespiti</li>
      <li>Model Sıkıştırma</li>
      <li>Bilgi Damıtma</li>
      <li>Gömülü Yapay Zeka</li>
      <li>RLHF</li>
      <li>CUDA Optimizasyonu</li>
    </ul>
  </div>

  <div class="about-section">
    <h2>Teknik Yığın</h2>
    <div class="stack-grid">
      <div class="stack-item"><span class="stack-dot"></span>PyTorch</div>
      <div class="stack-item"><span class="stack-dot"></span>TensorFlow</div>
      <div class="stack-item"><span class="stack-dot"></span>OpenCV</div>
      <div class="stack-item"><span class="stack-dot"></span>CUDA / C++</div>
      <div class="stack-item"><span class="stack-dot"></span>Python</div>
      <div class="stack-item"><span class="stack-dot"></span>MATLAB</div>
      <div class="stack-item"><span class="stack-dot"></span>Linux / Ubuntu</div>
      <div class="stack-item"><span class="stack-dot"></span>Git</div>
    </div>
  </div>

  <p class="open-to">
    Applied AI Research, Computer Vision Mühendisliği veya MLOps pozisyonlarına açığım.<br>
    İletişim: <a href="mailto:umutonuryasar@gmail.com">umutonuryasar@gmail.com</a>
  </p>

</div>

<script>
function setLang(lang) {
  document.querySelectorAll('.lang-content').forEach(el => el.classList.remove('visible'));
  document.querySelectorAll('.lang-btn').forEach(el => el.classList.remove('active'));
  document.getElementById('content-' + lang).classList.add('visible');
  event.target.classList.add('active');
}
</script>
