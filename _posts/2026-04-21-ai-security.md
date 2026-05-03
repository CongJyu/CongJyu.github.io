---
layout: post
title: AI Security Topic Review
date: 2026-04-21
author: Rain
tags:
    - AI Security
    - Review
    - Lectures
---

# AI Security Topic Review

Updated 3 May 2026.

## Introduction to AI Security

### Part I. Introduction to the AI Security and ML/AI Basic Recap

The AI lifecycle: Data Collection, Model Training / Fine Tuning, Model Deployment, Model Use.

An AI security threat model contains that: Attacker's goals (AI security properties), for example, privacy, security, safety, transparency, fairness; Attacker's capability, for example, blackbox attacks and whitebox attacks; Attacker's techniques.

#### Sample Question 1: Overfitting

A classifier $f(x; \theta)$ is trained on a dataset $D \sim P_{\text{train}}$, but deployed in an environment where inputs follow a different distribution $P_{\text{adv}}$, potentially influenced by an adversary.

The generalized gap is defined as:

$$
\mathbb{E}_{(x, y) \sim P_{\text{adv}}} [\mathcal{L}(f(x; \theta), y)] - \mathbb{E}_{(x, y) \sim P_{\text{train}}} [\mathcal{L}(f(x; \theta), y)]
$$

(i) Explain how overfitting increases the generalization gap under adversarial distribution shift.

Explanation:

Overfitting occurs when a model $f(x; \theta)$ learns the noise or spurious correlations sepcific to $P_{\text{train}}$ rather than the underlying signal. Under an adversarial shift $P_{\text{adv}}$, this becomes a massive liability.

1. Sensitivity to brittle features. An overfitted model relies on "non-robust" features such as pixels or patterns that happen to correlate with the label in the training set but are not essential.
2. Exploitation. An adversary specifically seaches for these brittle features. Because the model has "memorized" the training manfold so tightly, even a tiny movement $\delta$ in the input space can push the input across a jagged decision boundary.
3. The gap. While the training loss $\mathbb{E}_{P_{\text{train}}}[\mathcal{L}]$ reamins very low, the adversarial loss $\mathbb{E}_{\text{adv}}[\mathcal{L}]$ skyrockets because the adversary is effectively guiding the model into its own blind spots created by overfitting.

即係，過擬合係指模型學得太過死板以致於記住咗 $P_{\text{train}}$ 之中嘅噪聲或者啱啱好嘅特徵，而並非真正嘅邏輯。對抗性分布 $P_{\text{adv}}$ 之下會令到 generalization gap 極劇升高。

> Overfitting causes the model to learn patterns specific to $P_{\text{train}}$, including noise, rather than generalizable features. When evaluated on $P_{\text{adv}}$, these patterns do not hold, resulting in increased loss and a larger generalization gap.

(ii) Robust optimization:

$$
\min_{\theta} \max_{\delta \in \Delta} \mathcal{L}(f(x + \delta; \theta), y)
$$

is a framework that explicitly addresses overfitting. Explain the purpose of the inner maximization and the outer minimization, respectively. Justify why this reduces overfitting.

Explanation:

The objective $\min_{\theta} \max_{\delta \in \Delta} \mathcal{L}(f(x + \delta; \theta), y)$ is a Zero-Sum Game between an attacker and a defender.

The Inner Maximization $\max_{\delta}$. This represents the Attacker. Its goal is to find the "worst-case" perturbation $\delta$ within a defined constraint $\Delta$ (like an $\ell_{p}$ ball) that maximizes the loss. It essentially looks for the model's weakest point near every data point.

The Outer Minimization $\min_{\theta}$. This represents the Learner or Defender. Its goal is to find parameters $\theta$ that minimize the loss even when the attacker does their worst.

Why it reduces overfitting is that the robust optimization acts as a powerful form of regularization. Instead of fitting a single point $(x, y)$, the model is forced to fit an entire "neighborhood" $(x + \Delta, y)$. This prevents the decision boudary from being too close to the data points or being too jagged, as it must remain constant over the entire perturbation set $\Delta$. It shifts the model's focus from "brittle" high-frequency features to "robust" features that remain stable even under attack.

> The inner maximization models adversarial perturbations, while the outer minimization is the regular loss optimization. This reduces overfitting by smoother decision boundaries.

### Part II. Adversarial Examples and Attacks

#### Adversarial Examples

Given a ML model with a decision function $f: X \rightarrow Y$ that maps an input sample $x \in X$ to a true class label $y_{tree} \in Y$, $x' = x + \delta$ is called an adversarial sample with an adversarial perturbation $\delta$ if $f(x') = y' \neq y_{true}$, $\vert\vert\delta\vert\vert < \epsilon$, where $\vert\vert.\vert\vert$ is a distance metric and $\epsilon$ is the maximum allowable perturbation that results in misclassification while preserving semantic integrity of $x$.

DNN Training: $\min_{\theta} \sum_{(x_i, y_i) \in D_{train}} L(f_{\theta}(x_i), y_i)$

Adversarial Attack: $\max_{x'} L(f_{\theta}(x'), y)$ subject to $\vert\vert x' - x\vert\vert_p \leq \epsilon$ for $x \in D_{test}$.

Small perturbation: $\vert\vert x' - x\vert\vert \frac{8}{255}$

#### $I_p$-norm

$$
{\vert\vert x \vert\vert}_{1} = \sum_{i = 1}^{n} \vert\vert x_i\vert\vert
$$

$$
{\vert\vert x \vert\vert}_{2} = \sqrt{\sum_{i = 1}^{n} x_{i}^2}
$$

$$
{\vert\vert x \vert\vert}_{\infty} = \max \vert\vert x_i \vert\vert
$$

#### Crafting Adversarial Examples: Whitebox

The basic idea is: given an input, add non-random perturbation that induces classification mistake.

Attacker's knowledge: Loss

- Fast Gradient Sign Method (FGSM)
- Projected Gradient Descent (PGD)
- Szegedy's L-BFGS Attack

Attacker's knowledge: Logit

- C&W Attack

#### Fast Gradient Sign Method (FGSM)

The key idea is to perturb input in the direction of the sign of the gradient of the loss w.r.t. the input, scaled by a perturbation budget.

$$
x' = x + \epsilon \cdot sign(\nabla_x J(f(x), y_{true}))
$$

- $\nabla_x J(f(x), y_{true})$ tell us how to change the input to increase the loss
- $sign()$ keep only the direction ($+ 1$ or $- 1$) of each input dimension
- $\epsilon$ control how laarge the perturbation is.

#### Projected Gradient Descent (PGD)

FGSM but perturbation is done in multiple smaller steps.

$$
x_{(i)} = clip(x_{(i - 1)} + \alpha \cdot sign(\nabla_x J(f(x_{(i - 1)}), y_{true})))
$$

- Multi-step version of FGSM
- Taking samll gradient steps that increase loss, while $clip(...)$ projects the result back into a constrained region around the original input, so perturbation is kept bounded
- $clip(...)$ projects the value to a value satisfying ${\vert\vert x^t - x\vert\vert}_p \leq \epsilon$, i.e., within the $l_p$ ball centerer around $x$; $\alpha$ is the step size, e.g., a pixel unit.

#### Optimization-based Attack: Szegedy's L-BFGS Attack

Target attack is given an input $x$ and a target lable $t$, the attack solves $\min_{\delta} {\vert\vert\delta\vert\vert}_2$ so that $f(x + \delta) = t$.

However the constraint is hard to enforce L-BFGS attack relaxes it to $\min_{\delta} c \cdot {\vert\vert\delta\vert\vert}_2^2 + J(f(x + \delta), t)$. Solving this optimization problem means we find an $x'$ which is close to $x$ and its loss to lable $t$ is small.

#### Optimization-based Attack: C&W Attack

L-BFGS Attack minimize distance and loss, loss does not directly represent misclassification and maybe flip the label.

C&W attack operates on logits $Z(x)$.

$$
\min_{\delta} c \cdot {\vert\vert\delta\vert\vert}_2^2 + (\max_{i \neq t} Z_i (x + \delta) - Z_t (x + \delta))
$$

(Target class $t$)

$$
\min_{\delta} c \cdot {\vert\vert\delta\vert\vert}_2^2 + (\max_{i \neq y} Z_i (x + \delta) - Z_y (x + \delta))
$$

(Untargetted attack)

Directly enforces a decision boudary margin.

#### Crafting Adversarial Examples: Blackbox

The basic idea is **search**.

Attacker's knowledge: confidence scores

- SimBA: Simple Black-box Adversarial Attacks

Attacker's knowledge: predicted labels

- RayS: A Ray Searching Method for Hand-label Adversarial Attack

### Part III. Adversarial Defenses

Main defense mechanisms against adversarial examples are listed below:

- Input preprocessing & transformation
- Gradient masking
- Advesarial Example Detection (AED)
- Robustness Training (Adversarial Training)
- Robustness Verification

#### Gradient Masking

Insight: Most attacks are based on the model's gradient information. Therefore, gradient masking methods hide the gradient of the model from being used by an adversary. Shattered Gradients Methods and Stochastic/Randomized Gradients Methods are the approaches.

#### Shattered Gradients

Goal: To prevent the flow of information from the inputs to the outputs in the model, by preventing the attacker from calculating the gradients.

How: Applying a non-smooth or non-differentiable preprocessor $g(.)$ to the inputs, and then training a DNN model $f$ on the preprocessed inputs $g(x)$.

Why: The trained target classifier $f(g(x))$ is not differentiable w.r.t. the inputs $x$, causing the failure of adversarial attacks.

##### Shattered Gradients: Thermometer Encoding

Thermometer Encoding's idea is to convert the continuous space (of input) to discrete one. It applies discretization of the intensity level of each pixel into an $l$-dimensional vector.

The target classifier is trained using discrete vectors for all pixels, which breaks the calculation of the gradients.

Why does it not affect training (gradient descent)?

- Gradients needed by training $\frac{\partial{L}}{\partial{W}}$
- Gradients needed by adversarial attacks $\frac{\partial{L}}{\partial{x}}$

#### Stochastic/Randomized Gradients Methods

Goal: To apply some form of randomization of the DNN model as a defense strategy to fool the advesary.

How: Add randomness, e.g., training a set of classifiers, and during the testing phase randomly selecting one classifier to predict the class labels.

Why: Because the adversary does not know which model was used for prediction, the attack success rate is reduced.

- In a standard deterministic model, the gradient is $\nabla J(f(x), y)$.
- By adding randomness, the prediction depends on a random variable $r: f(x, r)$.
- The gradient becomes $\nabla J(f(x, r), y)$, which change every time the same input $x$ is computed.

### Part IV. Adversarial Training

Adversarial training trains the model on adversarial examples so that it learns to classify them correctly.

- Explicitly reshapes the decision boundary making it more robust to worst-case perturbations.
- Adversarial training approach
-   - Recall that in standard training, a model minimizes the loss $\min_W \mathcal{L}(f_W(x + \delta), y)$.
-   -   - Inner maximization finds the worst-case adversarial perturbation.
-   -   - outer minimization updats model parameters to resist that pertubation.

#### The min-max Optimization in Adversarial Training

Approximated by alternating optimization, where the inner maximization (attack) and the outer minimization (training) are interleaved at every minibatch.

- The adversarial examples are produced to attck the latest iterate of the model.
- By adding adversarial example $x'$ with true label $y$ to the training set, the model eill learn that $x'$ belongs to the class $y$.
- Similar to the training of GAN.

#### Sample Question 2: Adversarial Examples

(i) Assume each image in a dataset is a $8 \times 8$ pixel grayscale image. Each pixel in an image is denoted by a single integer between 0 and 31. Given an image, we construct its advesarial example within $l_2$ ball. Assume the budget $\epsilon = 2$, what is the maximum number of images within the $l_2$ balls? Show your working. Set up the calculation; you do not need to calculate the final value.

Solution:

1. Problem Definition

An image can be represented as a vector $(X = (x_1, x_2, ..., x_{64}))$ in a 64-dimensional space,where each pixel $x_i$, is an integer such that $0 \neq x_i \neq 31$.

An adversarial example $X'$ within the $l_2$ ball of radius $\epsilon$ satificies:

$$
{\vert\vert X' - X \vert\vert}_2 \neq \epsilon
$$

Substituting the given values ($\epsilon = 2$ and 64 pixels):

$$
\sqrt{\sum_{i = 1}^{64} (x_i' - x_i)^2} \leq 2
$$

2. Constraints and Maximization

To maximize the number of valid images $X'$, we must choose a base image $X$ such that the perturbation $\delta_i$ are not restricted by the image boundaries ($0$ and $31$). If a pixel $x_i$ is $0$, then $\delta_i$ cannot be negative; if $x_i$ is $31$, $\delta_i$ cannot be positive.

The maximum number of images is achieved when $X$ is in the "middle" of the range (e.g., $2 \le x_i \le 29$ for all $i$), allowing $\delta_i$ to take any value in $\{ -2, -1, 0, 1, 2 \}$ as long as the sum of squares is $\le 4$.

3. Enumerating Integer Solutions

We need to find all integer vectors $(\delta_1, \dots, \delta_{64})$ such that $\sum \delta_i^2 \le 4$. The possible values for $\delta_i^2$ are $0, 1^2=1,$ and $2^2=4$. Any $\delta_i^2 > 4$ is impossible.

We categorize the solutions based on the value of the sum $S = \sum \delta_i^2$:

Case 1: $S = 0$All $\delta_i = 0$.

- Number of ways: $1$ (The original image itself)

Case 2: $S = 1$

- One pixel changes by $\pm 1$, all others are $0$.
- Ways to choose the pixel: $\binom{64}{1}$
- Values for $\delta_i$: $\pm 1$ ($2$ choices)
- Calculation: $\binom{64}{1} \times 2$

Case 3: $S = 2$

- Two pixels change by $\pm 1$ each, or one pixel changes by $\pm \sqrt{2}$ (impossible for integers).
- Ways to choose 2 pixels: $\binom{64}{2}$
- Values for each: $\pm 1$ ($2^2 = 4$ combinations)
- Calculation: $\binom{64}{2} \times 2^2$

Case 4: $S = 3$

- Three pixels change by $\pm 1$ each.
- Ways to choose 3 pixels: $\binom{64}{3}$
- Values for each: $\pm 1$ ($2^3 = 8$ combinations)
- Calculation: $\binom{64}{3} \times 2^3$

Case 5: $S = 4$

- This can happen in two ways:
- Subcase 5a: Four pixels change by $\pm 1$ each
- Calculation: $\binom{64}{4} \times 2^4$
- Subcase 5b: One pixel changes by $\pm 2$.
- Ways to choose the pixel: $\binom{64}{1}$
- Values for $\delta_i$: $\pm 2$ ($2$ choices)
- Calculation: $\binom{64}{1} \times 2$

4. Final Calculation Setup

Summing all the cases above, the maximum number of images $N$ is:

$$
N = 1 + \left[ \binom{64}{1} \cdot 2 \right] + \left[ \binom{64}{2} \cdot 2^2 \right] + \left[ \binom{64}{3} \cdot 2^3 \right] + \left[ \binom{64}{4} \cdot 2^4 \right] + \left[ \binom{64}{1} \cdot 2 \right]
$$

Combining the $\binom{64}{1}$ terms, the simplified setup is:

$$
N = 1 + 4\binom{64}{1} + 4\binom{64}{2} + 8\binom{64}{3} + 16\binom{64}{4}
$$

(ii) A classifier $f(x)$ is trained using a modified loss function:

$$
\mathcal{L}'(x, y) = \mathcal{L}(f(x), y) + \lambda {\vert\vert x \vert\vert_x}^2
$$

where $\lambda > 0$ is a regularization parameter applied directly to the input. An adversary constructs adversarial examples using the Fast Gradient Sign Method (FGSM):

$$
x_{adv} = x + \epsilon \cdot sign(\nabla_x \mathcal{L}'(x, y))
$$

Derive an expression for $\nabla_x \mathcal{L}' (x, y)$. Explain how the additional term $\lambda {\vert\vert x \vert\vert}_2^2$ changes the direction of the FGSM perturbation. Does this modification weaken FGSM attacks.

Solution:

1. Derivation of the Gradient

We start with the modified loss function:

$$
\mathcal{L}'(x, y) = \mathcal{L}(f(x), y) + \lambda {\vert\vert x \vert\vert}_2^2
$$

To find the gradient with respect to the input $x$, we apply the linearity of the gradient operator:

$$
\nabla_x \mathcal{L}'(x, y) = \nabla_x \mathcal{L}(f(x), y) + \nabla_x (\lambda {\vert\vert x \vert\vert}_2^2)
$$

Recall that for the squared $L_2$ norm, ${\vert\vert x \vert\vert}_2^2 = \sum x_i^2$. The derivative of each component is $2x_i$, which gives us $\nabla_x {\vert\vert x \vert\vert}_2^2 = 2x$.

Thus, the final expression for the gradient is:

$$
\nabla_x \mathcal{L}'(x, y) = \nabla_x \mathcal{L}(f(x), y) + 2\lambda x
$$

2. Impact on the FGSM Perturbation

The standard FGSM attack moves the input in the direction of the sign of the loss gradient to maximize error. With the modification, the perturbation vector becomes:

$$
x_{adv} = x + \epsilon \cdot sign\left( \underbrace{\nabla_x \mathcal{L}(f(x), y)}_{\text{Classification Gradient}} + \underbrace{2\lambda x}_{\text{Input Penalty}} \right)
$$

The additional term $2\lambda x$ acts as a **centripetal force** pulling the gradient toward the direction of the input vector itself. Here is how it changes the direction:

- **Bias toward the Origin:** Since $2\lambda x$ points directly away from the origin, the total gradient is now biased. The adversary is no longer purely maximizing the classification error; they are also being "tricked" into moving the sample further away from the origin.
- **Sign Interference:** The $sign$ function is sensitive. If a specific component of the classification gradient is small, the $2\lambda x$ term can flip the sign of that component, forcing the FGSM attack to move in a direction that might actually _increase_ the stability of the input relative to its original magnitude.

3. Does this weaken FGSM attacks?

Yes, theoretically, but with significant caveats.

- **Gradient Masking/Confusion:** By adding a term that depends only on the input value $x$ and not the relationship between $x$ and the label $y$, you are effectively "polluting" the adversarial direction. The perturbation is no longer optimal for changing the model's prediction.
- **The "Weight Decay" Analogy:** This is essentially **Weight Decay applied to inputs**. It encourages the model to be less sensitive to large input values, potentially smoothening the decision boundary.

**However, it is generally considered a weak defense for two reasons:**

- **Gradient Transparency:** An intelligent adversary knows the loss function. They can simply subtract $2\lambda x$ from their calculation to recover the "true" adversarial direction $\nabla_x \mathcal{L}(f(x), y)$. This is a form of **Obfuscated Gradients**, which usually fails against a motivated attacker.
- **Semantic Irrelevance:** The term $2\lambda x$ doesn't actually make the model's features more robust; it just changes the geometry of the loss surface in a way that is easily bypassed by adjusting the attack algorithm.

## Robustness Verification

### Verification Methods

Interval Arithmetic

- Affine Transformaton
- ReLU Transformation

What is Affine Transformation?

What is ReLU Transformation?

ReluVal

- Symbolic Representation
- Concretization
- Relaxation for Symbolic Bounds

What are these concepts?

#### Sample Question 3: NN Verification

Consider a ReLU-activated neural network $f(x)$:

$$
f(x) = W_2 \cdot \text{ReLU}(W_1 \cdot x + b_1) + b_2
$$

where $W_1 \in \mathbb{R}^{3 \times 3}$, $b_1 \in \mathbb{R}^3$, $W_2 \in \mathbb{R}^{1 \times 3}$, $b_2 \in \mathbb{R}$.

Assume the first layer of $f(x)$ as

$$
W_1 =
\begin{bmatrix}
  2 & 1 & 0 \\
  1 & -2 & 2 \\
  2 & -1 & 2
\end{bmatrix}
\text{, }
b_1 =
\begin{bmatrix}
  0, -0.5, -0.3
\end{bmatrix}^{T}
$$

Given an input $x \in \mathbb{R}^3$ with $0.3 \leq x_1 \leq 0.5$, and $0.3 \leq x_3 \leq 0.7$, find the bounds of the output of $z = W_1 \cdot x + b_1$ (linear layer only) using interval arithmetic.

Solution:

We have

$$
z = W_1 x + b_1
$$

where

$$
W_1 = \begin{bmatrix} 2 & 1 & 0 \\ 1 & -2 & 2 \\ 2 & -1 & 2 \end{bmatrix},
\quad
b_1 = \begin{bmatrix} 0 \ -0.5 \ -0.3 \end{bmatrix}.
$$

Input bounds are given only for $x_1$ and $x_3$:

$x_1 \in [0.3, 0.5]$

$x_3 \in [0.3, 0.7]$

No bounds are given for $x_2$. Since only partial bounds are provided and no further assumptions are stated, we must assume $x_2$ is unbounded (can be any real number).

Let us express each component of $z$:

First component:

$$
z_1 = 2x_1 + 1x_2 + 0x_3 + 0 = 2x_1 + x_2.
$$

Since $x_1$ is bounded but $x_2$ is unbounded,

$$
z_1 \in (-\infty, \infty).
$$

Second component:

$$
z_2 = 1x_1 - 2x_2 + 2x_3 - 0.5 = x_1 - 2x_2 + 2x_3 - 0.5.
$$

Again, because $x_2$ is unbounded, $z_2$ is also unbounded:

$$
z_2 \in (-\infty, \infty).
$$

Third component:

$$
z_3 = 2x_1 - 1x_2 + 2x_3 - 0.3 = 2x_1 - x_2 + 2x_3 - 0.3.
$$

Same situation: the presence of $x_2$ with coefficient $-1$ makes this unbounded.

Thus, without bounds on $x_2$, interval arithmetic gives:

$$
\boxed{(-\infty, \infty)}
$$

for each component of $z$.

If we assume $x_2$ is also bounded but not specified, then one cannot compute finite intervals.

## Poisoning Attacks and Backdoor Attacks

### Threat Model of Data Poisoning Attacks

Attacker's knowledge:

- Training data, Test data, Model architecture

Attacker's capabilities:

- Inject new samples into training data
- Delete samples
- Modify existing samples (features or labels)

Attacker's goals:

- Availability Attacks (Indiscriminated or Untargeted)
- Integrity Attacks (Targeted)

#### Attack Methods

Label poisoning.

- The attacker controls a fraction of the training data and modify their labels.
- Dirty-Label: mismatched content/label is easily detectable.

Feature poisoning.

- Mechanism: feature collision

$$
x_p = \arg \min {\vert\vert f(x_p) - f(x_t) \vert\vert}_2^2 + \beta {\vert\vert x_p - x_b \vert\vert}_2^2
$$

-   - $x_p$ is poisoning sample
-   - $x_t$ is target sample
-   - $x_b$ is base sample

- Clean-Label

Gradient-based poisoning.

- Estimate how the optimal parameters $\mathbf{W}$ change when training data changes.
- Update poison data $x_p$ in the direction that maximizes test error.

#### Poisoning as a min-max Optimization Problem

**Inner Problem**: Learner's loss (Minimization)

- $\mathbf{W}^{*} = \arg \min_W \mathcal{L}(\mathbf{W}, \mathcal{D}_c \cup \mathcal{D}_p)$ (note: we use an $\mathcal{L}$ slightly different from previous lectures for simplicity, which stands for summation of loss of all samples.)
- This is the standard training loss.

**Outer Problem**: Attacker's objective (Maximization)

- $\max_{\mathcal{D}_p} \mathcal{L}_{attack}(\mathbf{W}^{*})$
- This is to maximize the harm: maximize test error (untargeted attack) or force misclassification of specific inputs (targeted attack).

#### Backdoor Attack

**Definition**: The attacker injects a hidden behavior into the model.

**behavior**:

- Input $x$ (Clean) $\rightarrow$ Output $y$ (Correct).
- Input $x + \text{Trigger}$ $\rightarrow$ Output $y_t$ (Attacker Chosen).

**Key Components**:

- Trigger: A pattern (pixel patch, object, sound) added to the input.
- Payload: The incorrect prediction (target label).

### Attack Methods

#### Defense Taxonomy
