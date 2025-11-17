---
layout: post
title: Paper Reading - Open-Sora 2.0
date: 2025-11-14
author: Rain
tags:
- Artificial Intelligence
- Papers
---

# Paper Reading - Open-Sora 2.0: Training a Commercial-Level Video Generation Model in $200k

This paper is written by Open-Sora Team.

## Motivation

Video generation models have achieved remarkable progress in the pass year and the quality of the generated videos are also improved. But the costs and data quantity of these models increased, and it demands a greater training compute. The author presented a commercial-level video generation model that only costs $200k.

To evaluate the Open-Sora model, the team has designed a comparing table, which covers three key aspects:

- Visual quality
- Prompt adherence
- Motion quanlity

## Model Architecture

There are two key components of a video generation model: the autoencoder and the diffusion transformer. For the autoencoder, Open-Sora is initially trained on HunyuanVideo's VAE and later adapt to the Video DC-AE.

### 3D Autoencoder

While training this model, an open source video generation model HunyunVideo VAE is leveraged, and the team developed a video autoencoder with deep compression to improve efficiency and decrease the costs while maintaining high reconstruction fidelity, which is called "Video DC-AE".

The overview of Video DC-AE: Each block in encoder introduces spatial downsampling, while temporal downsampling occurs at blocks 4 and 5, with a corresponding symmetric structure in the decoder. The Video DC-AE encoder consists of three residual blocks followed by three EfficientViT blocks, with the decoder adopting a symmetrical structure.

## Why the 3 main stages in training process structured this way?

Open-Sora 2.0 uses a **three-stage training pipeline** to achieve commercial-level video generation quality (comparable to models like HunyuanVideo and Runway Gen-3 Alpha) at dramatically lower cost (~$200k total, 5–10× cheaper than comparable models). The structure prioritizes efficiency by doing the heavy lifting (learning diverse motion patterns and semantics) at low resolution with cheaper compute, leveraging strong pre-trained image models, and reserving expensive high-resolution training for a short final fine-tuning phase.

This avoids the prohibitive costs of training directly on high-resolution data from scratch, where attention complexity scales quadratically with token count/resolution, and minimizes token processing via a custom Video Deep Compression Autoencoder (Video DC-AE).

### Stage 1: Low-Resolution Text-to-Video (T2V) Pretraining (256px)
- **What happens**: The model (11B parameters, Diffusion Transformer-based) initializes from Flux (a strong open-source text-to-image model). It trains as text-to-video on ~70M carefully curated 256px short video clips for 85k iterations.
- **Cost**: ~$107.5k (most of the budget).
- **Why this stage first**: Low resolution keeps token counts and attention costs very low, allowing the model to efficiently learn diverse real-world motion patterns and text-video alignment on a massive scale. Starting from a pretrained T2I model (Flux) massively accelerates convergence instead of training video dynamics from scratch. The paper states: "we first train on 256px resolution videos, allowing the model to learn diverse motion patterns efficiently."

### Stage 2: Low-Resolution Image-to-Video (I2V) Adaptation (256px)
- **What happens**: Continues from Stage 1 checkpoint, shifts to image-to-video conditioning (first frame provided as input), training on a higher-quality subset of ~10M 256px clips for 13k iterations.
- **Cost**: ~$18.4k.
- **Why switch to I2V here**: Image conditioning lets the model reuse powerful pretrained image features (especially from Flux) and focus purely on temporal/motion modeling. This sets up much more efficient resolution upscaling later. The authors note that "adapting a model from 256px to 768px resolution is significantly more efficient using an image-to-video approach" because I2V avoids regenerating the first frame from text at high res (which is harder/less stable) and instead concentrates compute on coherent motion given a sharp starting image.

### Stage 3: High-Resolution Image-to-Video Fine-Tuning (768px)
- **What happens**: Fine-tunes the Stage 2 checkpoint as I2V (with text+image conditioning supported in the final model) on ~5M strictly filtered high-quality 768px videos for 13k iterations, using the Video DC-AE for extreme compression (4× temporal × 32× spatial = 4×32×32 overall, reducing tokens ~16× vs standard VAEs).
- **Cost**: ~$73.7k.
- **Why only fine-tune at high res, and why I2V**: High resolution is computationally brutal (much higher token counts), so they limit it to a short, targeted phase with a smaller dataset. Motion priors already learned in low-res stages transfer well, and I2V focuses effort on refining dynamics/perceptual quality rather than re-learning static composition. The paper emphasizes: "increasing the resolution significantly improves perceptual quality," but doing it early would explode costs.

### Overall Rationale for the 3-Stage Structure
The core insight is progressive/curricular training: learn motion cheaply at low res → adapt to stronger image conditioning → polish at high res only. This + Flux initialization + Video DC-AE compression gives massive savings while preserving quality stays on par with models trained for millions of dollars. Direct high-res T2V training from scratch, or doing everything in one stage, would require far more GPU time because of quadratic scaling and poorer convergence. By decoupling motion learning (low-res) from visual fidelity (high-res fine-tune) and using I2V for the expensive parts, they achieve 5.2× training throughput gains and >10× inference speedup versus less-compressed alternatives.

In short, the staging is an elegant cost–quality optimization that proves you don't need million-dollar runs to reach the frontier — you just need to train smart.
