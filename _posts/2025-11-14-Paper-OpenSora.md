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


