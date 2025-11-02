---
layout: post
title: Intelligent System - Notes and Practice
date: 2025-10-30
author: Rain
tags:
- CityU
- Intelligent System
- Artificial Intelligence
- Neural Network
---

# Intelligent System - Notes and Practice

> Intelligence is a mental quality that consists of the abilities to learn from experience, adapt to new situations, understanding and handle abstract concepts, and use knowledge to manipulate one's environment.
> 
> -- Britannica

### A Definition of Intelligent Systems

A system is an intelligent system if the system exhibits some intelligent behaviors. (e.g. fuzzy systems, simulated annealing, genetic algorithms and expert systems.)


## Support Vector Machines (SVM)


## つづく...

---

Based on the provided citations (CS5486 slides 1–220), here’s a concise **review note** to help you prepare for your exam. The content spans foundational topics in machine learning, neural networks, and optimization — especially SVMs, perceptrons, ADALINE, and evolutionary algorithms.

---

## 📘 **Exam Review Note: CS5486 (First 220 Pages)**

### 1. **Support Vector Machines (SVM) – Core Concepts**
- **Maximal Margin Classifier** → Better generalization, less overfitting.
- **Kernel Trick** → Maps data to higher dimensions without explicit transformation (only dot products used).
- **Kernel Selection & Parameters**:
  - Domain experts can help design similarity measures.
  - Gaussian kernel: σ = distance between closest points of different classes.
  - If no reliable criterion → use **cross-validation** or **validation set**.
- **Hard Margin vs. Soft Margin**:
  - Hard margin: strict separation → only works if data is perfectly separable.
  - Soft margin: allows some misclassification → more practical, especially with noisy data.

---

### 2. **Support Vector Regression (SVR) & Least-Squares SVM**
- SVR: Extension of SVM for regression.
- **Least-Squares SVM (LS-SVM)** by Suykens & Vandewalle (1999):
  - Primal problem → Lagrangian → Final model with closed-form solution.
  - Often faster than traditional SVM for regression.

---

### 3. **Neural Network Basics**
#### ➤ Perceptrons
- **Limitations**:
  - Can only classify **linearly separable** data.
  - Slow convergence in high dimensions.
- **Bipolar vs Unipolar**:
  - Bipolar ([-1, 1]) better for algebraic structure and weight space representation.

#### ➤ ADALINE
- Adaptive Linear Element (Widrow-Hoff, 1960s).
- Trained via **Delta Rule / LMS Algorithm**.
- Learning rule: updates weights to minimize error (gradient descent).
- Training modes: Sequential, Batch, or Perceptron-style.

#### ➤ MAXNET
- Winner-Takes-All (WTA) architecture.
- Uses self-excitatory & lateral-inhibitory connections.
- Often used as output layer to select max input → "winner takes all".
- Proven to be **globally stable and convergent** (Julian kWTA model).

---

### 4. **Optimization & Global Search**
#### ➤ Global Optimization
- Search space: defined by min/max bounds of variables.
- Schwefel’s function: benchmark for optimization (non-convex, multimodal).
- **Evolutionary Algorithms (e.g., PSO, GA)**:
  - Swarm evolution shown over iterations (0 → 500).
  - Converged to global optimum: ~837.9658 (example value).
- Optimization is critical for hyperparameter tuning (e.g., σ, C, kernel choice).

---

### 5. **Logic & Learning Theory**
- **Two-Variable Logic Functions** (OR, AND, Majority, XOR).
- **XOR Problem**: Classic non-linearly separable problem → **cannot be solved by single-layer perceptron** → needs multi-layer networks (e.g., MLP).
- **Monte Carlo Tests**: Used to evaluate robustness of learning algorithms.

---

## 🧠 **Key Takeaways for Exam**
✔️ Understand the **difference between hard/soft margin SVMs**.  
✔️ Know how to **choose kernel parameters** (σ, etc.) and when to use cross-validation.  
✔️ Be able to **compare Perceptron, ADALINE, and MAXNET** — strengths, weaknesses, use cases.  
✔️ Understand the **math behind LMS and gradient descent** for ADALINE.  
✔️ Know why **SVMs can handle non-linear data** via kernel trick.  
✔️ Be familiar with **optimization landscapes** (Schwefel’s function, evolutionary search).  
✔️ Know **why XOR can’t be solved by single-layer perceptrons**.

---

## 📚 **Recommended Focus Areas**
- SVMs (Primal, Dual, Kernels, Hard vs Soft)
- Neural Networks (Perceptron, ADALINE, MAXNET)
- Optimization (Gradient descent, global search)
- Logic functions (XOR, Majority, OR/AND)
- Learning Algorithms (LMS, Delta Rule)

---

✅ **Good luck on your exam!**  
You’ve got this — especially if you’ve reviewed the slides up to page 220. Focus on the *why* behind the algorithms, not just the math.

--- 
