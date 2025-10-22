---
layout: post
title: Weird Python
date: 2025-10-19
author: Rain
tags:
- Python
- Coding
- Debug
---

# Weird Python

## Introduction

I create this article because of the annoying confusing problems I met in the Python code. I have to admit that Python is a awesome programming language however... it DOSE exist some weird problems when coding not carefully. So this passage is used to record the problems and solutions for Python debugging.

## Stable Method for Patching Functions in Packages

### Problem

Well, here's the problem. It happened when I was completing my tutorials. The tutorial imported a Python package called `lucent`, which is used to visualize the layers of CNNs. The code was like this:

```python
from lucent.optvis import render
import lucent.modelzoo.util as lu_zoo
```

It seems normal, but it may run into trouble because I will run this code on Jupyter Notebook (local or online). Then I use the `lucent` package to visualize the layers.

```python
model1.to(ldevice).eval()
lu_zoo.get_model_layers(model1)

model1_conv_images = []
model1_conv_names = [
    "conv5:0", "conv5:1", "conv5:3", "conv5:7",
    "conv5:15", "conv5:31", "conv5:63", "conv5:127"
]

lu_zoo.get_model_layers(model1)

for x in model1_conv_names:
    out = render.render_vis(model1, x, show_image=False)
    model1_conv_images.append(np.squeeze(out[0]))

plt.figure(figsize=(8, 4))
show_imgs(model1_conv_images, nc=4)
```

Then the output cell run into trouble. There should be a beautiful progress bar loading in the output cell showing the current progress of the analyzation, however, the progress bar flashed and disappeared suddenly, leaving some static time text in the output cell. Like the cell below.

```
5it/s

second

second

second
```

The situation is not what we want.

### Attempts & Solution



### Summary



## つづく...
