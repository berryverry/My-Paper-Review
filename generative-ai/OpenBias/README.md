# ✨ OpenBias: Open-set Bias Detection in Text-to-Image Generative Models

Paper Review:  
[**"OpenBias: Open-set Bias Detection in Text-to-Image Generative Models" (arXiv:2404.07990v2)**](https://arxiv.org/abs/2404.07990v2)

---

## 📌 Overview

**Goal**: Introduce a bias detection pipeline for text-to-image (T2I) generative models that goes beyond closed-set assumptions.  
Rather than relying on predefined bias labels, OpenBias identifies emergent bias types directly from the prompts using large language models (LLMs), enabling detection in open-world settings.

**Key Contributions**:

- ❌ No pre-defined bias list needed
- ✅ Uses LLM to generate candidate bias types from captions
- ✅ Uses VQA models (e.g., Llava-13B) to assess bias presence in generated images
- ✅ Evaluated across Stable Diffusion 1.5, 2, and XL
- ✅ Demonstrated strong agreement with human evaluation & classifier-based benchmarks

---

## 🔍 Methodology: Pipeline & Technical Details

```
Prompt Captions
   │
   ├──> [Bias Proposal Module]
   │     ├─ Uses LLM (e.g., GPT-4) to extract potential bias attributes from the caption
   │     ├─ Outputs:
   │     │   • bias name (e.g., gender, race)
   │     │   • class candidates (e.g., "man", "woman")
   │     │   • diagnostic question (e.g., "Is the person a woman?")
   │
   ├──> [Image Generation Module]
   │     └─ Stable Diffusion (v1.5, v2, XL) used to generate images from captions
   │
   └──> [Bias Assessment Module]
         ├─ Applies VQA model to generated images + diagnostic questions
         ├─ Returns probability/confidence scores for each candidate class
         └─ Computes Bias Score
```

**Highlights**:

- The pipeline supports both **context-aware** (bias measured within caption-specific context) and **context-free** (bias measured in general image distribution) modes of bias evaluation
- LLMs enable dynamic and flexible definition of bias axes per prompt — a powerful step beyond static bias categories

---

## 💬 Reflection: What This Paper Meant to Me

What impressed me most was the pipeline’s compositional design: it lets each model (LLM, diffusion, VQA) do what it does best, but in service of a greater interpretability goal. That modularity — not just as a system architecture, but as a way of thinking about bias — felt refreshing.

From my background in multimodal reasoning and computer vision, this method highlighted the importance of **emergent bias detection** in generative pipelines, which is increasingly relevant for deploying models in open-world settings.

Having worked on representation robustness and generalization in vision-language models, I resonated with the challenge of **detecting bias without predefined bias labels**, especially when concepts are culturally or socially complex.

---

## 🔗 Broader Research Fit

This work reflects a broader trend toward:

- **Modular interpretability** using VLMs and LLMs together
- **Bias and fairness detection** without supervision or labeled bias datasets
- **Adaptive learning systems** that remain robust under shift

These are themes that continue to shape my interest in trustworthy machine learning — particularly in how we can build systems that are more reflective, less rigid, and more generalizable.

---

## ⚠️ Limitations I Noticed

OpenBias is thoughtfully designed, but several challenges remain:

- **Prompt Sensitivity**: Bias proposals are LLM-generated, which introduces variability depending on phrasing or LLM version
- **VQA Model Trust**: VQA model answers can be noisy or fail to align with human intuition, especially for subtle or ambiguous visual cues — answers may be technically correct but semantically off
- **Caption-to-Image Drift**: There's no explicit mechanism for verifying semantic alignment between caption intent and generated image content

---

## 💡 Ideas This Paper Sparked

- Replace raw LLM output with **rule-augmented prompting** or structured templates for more stable bias proposals
- Explore OpenBias for **longitudinal bias tracking** in continual learning settings
- Extend pipeline to **video diffusion models** — how does visual bias evolve over time?
