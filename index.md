---
layout: project_page
permalink: /

title: "A Semantically Disentangled Unified Model for Multi-category 3D Anomaly Detection"
affiliations:
    Kyung Hee University
arxiv: https://arxiv.org/pdf/2603.09611 # change to CVF
paper: https://arxiv.org/pdf/2603.09611 # change to CVF
code: https://github.com/VisualScienceLab-KHU/SeDiR # change to CVF
---

![Figure1](./static/image/figure1.png)

<div class="columns is-centered has-text-centered">
  <div class="column is-four-fifths">
    <h2>Abstract</h2>
    <div class="content has-text-justified">
      3D anomaly detection targets the detection and localization of defects in 3D point clouds trained solely on normal data. While a unified model improves scalability by learning across multiple categories, it often suffers from <b>Inter-Category Entanglement (ICE)</b>—where latent features from different categories overlap, causing the model to adopt incorrect semantic priors during reconstruction and ultimately yielding unreliable anomaly scores. To address this issue, we propose the <b>Semantically Disentangled Unified Model</b> for 3D Anomaly Detection, which reconstructs features conditioned on disentangled semantic representations. Our framework consists of three key components: (i) <b>Coarse-to-Fine Global Tokenization</b> for forming instance-level semantic identity, (ii) <b>Category-Conditioned Contrastive Learning</b> for disentangling category semantics, and (iii) a <b>Geometry-Guided Decoder</b> for semantically consistent reconstruction. Extensive experiments on Real3D-AD and Anomaly-ShapeNet demonstrate that our method achieves state-of-the-art for both unified and category-specific models, improving object-level AUROC by 2.8% and 9.1%, respectively, while enhancing the reliability of unified 3D anomaly detection. 
    </div>
  </div>
</div>


<h2 style="text-align: center;">Architecture</h2>

### Stage 1. Temporal-aware Vector Quantization

<div class="columns is-vcentered">
  <div class="column is-6">
    <img src="./static/image/figure2.png" alt="figure2" style="width: 100%;">
  </div>
  <div class="column is-6">
    <b>Temporal-aware VQ-VAE</b> reduces temporal information loss caused by fixed-window quantization. It first applies <b>Local Temporal Enhancement (LTE)</b>, where an MLP-based weighted sum preserves important short-term motion cues within each window. It then applies <b>Global Temporal Enhancement (GTE)</b>, where a GCN updates group-level features to capture long-range temporal dependencies. The enhanced features are mapped to codebook tokens, and the same design is used for both full-body and part (arms/legs) streams to provide stable motion tokens for next stage.
  </div>
</div>

### Stage 2. Part-Guided Motion Synthesis

<p align="center">
  <img src="./static/image/figure3.png" width="100%" alt="figure3">
</p>

Next, we first applies <b>Part-aware Text Grounding (PTG)</b>: a CLIP text embedding is transformed by multiple part-specific MLPs, and a part gate selects the most suitable embedding for each body part. To keep both diversity and semantic consistency, PTG is trained with a contrastive diversity objective and an auxiliary part-text alignment loss.
Then the <b>Part-Guided Network</b> generates arm/leg tokens autoregressively and fuses them into <b>Part Guidance</b> over short cycles. The holistic transformer uses this guidance to generate full-body tokens, instead of predicting holistic motion alone.
During generation, <b>Holistic-Part Fusion (HPF)</b> continuously injects part tokens into the holistic stream via attention, improving whole-body coordination while preserving fine-grained part expressiveness.

<br>

<h2 style="text-align: center;">Visualizations</h2>

<p align="center">
  <img src="./static/image/figure4.png" width="100%" alt="figure4">
</p>

**Part-level evaluation.** The qualitative analysis shows that ParTY better matches fine-grained part instructions (e.g., specific arm/leg actions) than both holistic (MoMask) and prior part-wise baseline (ParCo). This observation is consistent with the part-level metrics: ParTY improves part-wise text-motion alignment and motion quality for both arms and legs, indicating that part semantics are preserved during generation rather than diluted in full-body synthesis.

**Coherence-level evaluation.** We also highlights that strong part control alone is insufficient if global coordination collapses. Prior part-wise generation can show artifacts such as neck distortion or mismatched upper/lower body orientation, which lowers temporal and spatial consistency. ParTY maintains synchronized full-body dynamics while executing part-specific motions, reflected by stronger <b>Temporal Coherence (TC)</b> and <b>Spatial Coherence (SC)</b> scores and visually stable poses across frames.

<br>

## BibTeX
