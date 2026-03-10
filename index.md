---
layout: project_page
permalink: /

title: "ParTY: Part-Guidance for Expressive Text-to-Motion Synthesis"
affiliations:
    Kyung Hee University
arxiv: https://arxiv.org/pdf/2510.04714 # change to arxiv
paper: https://arxiv.org/pdf/2510.04714 # change to CVF
code: https://github.com/VisualScienceLab-KHU/ParTY
---

![Figure1](/static/image/figure1.png)

<div class="columns is-centered has-text-centered">
  <div class="column is-four-fifths">
    <h2>Abstract</h2>
    <div class="content has-text-justified">
      Text-to-motion synthesis aims to generate natural and expressive human motions from textual descriptions. While existing approaches primarily focus on generating holistic motions from text descriptions, they struggle to accurately reflect actions involving specific body parts. Recent part-wise motion generation methods attempt to resolve this but face two critical limitations: (i) they lack explicit mechanisms for aligning textual semantics with individual body parts, and (ii) they often generate incoherent full-body motions due to integrating independently generated part motions. To overcome these issues and resolve the fundamental trade-off in existing methods, we propose <b>ParTY</b>, a novel framework that enhances part expressiveness while generating coherent full-body motions. ParTY comprises: <b>(1) Part-Guided Network</b>, which first generates part motions to obtain part guidance, then uses it to generate holistic motions; <b>(2) Part-aware Text Grounding</b>, which diversely transforms text embeddings and appropriately aligns them with each body part; and <b>(3) Holistic-Part Fusion</b>, which adaptively fuses holistic motions and part motions. Extensive experiments, including <b>part-level</b> and <b>coherence-level</b> evaluations, demonstrate that ParTY achieves substantial improvements over previous methods.
    </div>
  </div>
</div>

<br>

## Architecture

### Stage 1. Temporal-aware Vector Quatization

<div class="columns is-vcentered">
  <div class="column is-7">
    <img src="/static/image/figure2.png" alt="figure2">
  </div>
  <div class="column is-5">
    Temporal-aware VQ-VAE reduces temporal information loss caused by fixed-window quantization.
    It first applies Local Temporal Enhancement (LTE), where an MLP-based weighted sum preserves important short-term motion cues within each window.
    It then applies Global Temporal Enhancement (GTE), where a GCN updates group-level features to capture long-range temporal dependencies.
    The enhanced features are mapped to codebook tokens, and the same design is used for both full-body and part (arms/legs) streams to provide stable motion tokens for Stage 2.
  </div>
</div>

### Stage 2. Part-Guided Motion Synthesis

<div style="text-align: center;">
  <img src="/static/image/figure3.png" width="80%" alt="figure3">
</div>

<div class="content has-text-centered">
  Stage 2 first applies Part-aware Text Grounding (PTG): a CLIP text embedding is transformed by multiple part-specific MLPs, and a part gate selects the most suitable embedding for each body part. To keep both diversity and semantic consistency, PTG is trained with a contrastive diversity objective and an auxiliary part-text alignment loss.
  Then the Part-Guided Network generates arm/leg tokens autoregressively and fuses them into Part Guidance over short cycles. The holistic transformer uses this guidance to generate full-body tokens, instead of predicting holistic motion alone.
  During generation, Holistic-Part Fusion (HPF) continuously injects part tokens into the holistic stream via attention, improving whole-body coordination while preserving fine-grained part expressiveness.
</div>

<br>

## Visualizations

<div style="text-align: center;">
  <img src="/static/image/figure4.png" width="80%" alt="figure4">
</div>

<div class="content has-text-justified">
  <p><b>Part-level evaluation.</b> Following the qualitative analysis in Sec. 4.3, Fig. 4 shows that ParTY better matches fine-grained part instructions (e.g., specific arm/leg actions) than both holistic and prior part-wise baselines. This observation is consistent with the part-level metrics: ParTY improves part-wise text-motion alignment and motion quality for both arms and legs, indicating that part semantics are preserved during generation rather than diluted in full-body synthesis.</p>

  <p><b>Coherence-level evaluation.</b> Sec. 4.3 also highlights that strong part control alone is insufficient if global coordination collapses. In Fig. 4, prior part-wise generation can show artifacts such as neck distortion or mismatched upper/lower body orientation, which lowers temporal and spatial consistency. ParTY maintains synchronized full-body dynamics while executing part-specific motions, reflected by stronger TC and SC scores and visually stable poses across frames.</p>
</div>

<br>

## BibTeX
