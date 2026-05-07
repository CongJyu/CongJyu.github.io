---
layout: post
title: Autonomous Driving Review
date: 2026-05-04
author: Rain
tags:
    - Autonomous Driving
    - Review
    - Lectures
---

# Autonomous Driving Review: Problems and explanations

## Multiple Choice 1

### Problem Descriptions

What is a key weakness of data-driven track-based prediction?

A. Over-reliance on hand-engineered physical models.

B. Inability to use HD Map context.

C. High computational cost and low explainability.

D. Cannot handle partial observation scenarios.

### Solution

C.

### Explanations

For high computational cost, unlike simple kinematic models like Constant Velocity or Constant Steering Angle, data-driven models, such as LSTMs, Transformers, or Graph Neural Networks, require substantial memory and processing power. In the context of an autonomous vehicle, where predictions must be updated every few milliseconds, this "compute tax" is a major hurdle.

For low explainability, data-driven models operates as black boxes. If a model predics that a cyclist will suddenly veer into traffic, it is often impossible to trace the specific logic behind that decision within the millions of neural network weights. This makes it difficult to validate for safety-critical applications compared to model-based approaches where you can see the physical vectors.

Why option A is inccorect? Over-reliance on hand-engineered physical models, this is actually a weakness of model-based prediction, not data-driven prediction. Data-driven models specifically aim to move away from hand-engineered rules.

Why option B is inccorect? Inability to use HD Map context. Many data-driven tracked-based models like VectorNet are specifically designed to fuse agent tracks with HD map data. It is a strength of modern data-driven architectures, not a inherent weakness.

Why option D is inccorect? Cannot handle partial observation scenarios. Modern data-driven models are actually quite robust at "filling in the gaps" using learned temporal patterns via Recurrent Neural Networks or Attention mechanisms, often performing better than rigid physical models when sensor data is noisy or occluded.

## Multiple Choice 2

### Problem Descriptions

What is a key advantage of the linear quadratic regulator (LQR) compared to PID control?

A. Requires no tuning of parameters.

B. Optimizeds a quadratic cost function for linear systems (optimal control).

C. Works without a mathematical model of the system.

D. Is computationally cheaper for real-time deployment.

### Solution

B.

### Explanations

While PID control is a model-free apprach based on trial and error (tuning gains), LQR is a model-based approach that uses the physics of the system to find the mathematically best possible control strategy.

LQR is the gold standard for Optimal Control. It works by minimizing a specific quadratic cost function $J$.

$$
J = \int_{0}^{\infty} (x^T Q x + u^T R u)dt
$$

$x^TQx$ represents the penalty for the state being away from the target (error).

$u^TRu$ represents the penalty for using control effort (energy/actuation).

By adjusting the $Q$ and $R$ matrices, an engineer can mathematically decide the exact trade-off between how fast the system reaches the target versus how much energy it consumes. PID control cannot provide this "guaranteed optimality" because it does not know the physics of the system.

Why other choices are not correct?

For A, requires no tuning of parameters, this is false. While we do not tune $P$, $I$, and $D$ gains directly, we must tune the weighting matrices ($Q$ and $R$).

For C, works without a mathematical model, this is inccorect. LQR requires a precise State-Space Model (in the form of $\dot{x} = Ax + Bu$). PID is the one that can work without a model.

For D, is computational cheaper, which is generally false. PID is a very simple calculation. LQR requires solving the Algebraic Riccati Equation and performing matrix multiplications online. While modern computers handle both easily, LQR is technically more complex.

In summary,

| Feature | PID Control | LQR Control |
| --- | --- | --- |
| Model Needed? | No (Model-free) | Yes (State-space model) |
| Optimality? | No (Heuristic) | Yes (Mathematically optimal) |
| Handling MIMO? | Difficult (needs multiple loops) | Natural (handles multiple inputs/outputs) |
| Complexity | Low | Moderate |

## Multiple Choice 3

### Problem Descriptions

In closed-loop learning for planning, what is the key advantage over open-loop learning?

A. No need for expert demonstration data.

B. Eliminates distribution shift between training and inference.

C. Lower computational cost during training.

D. Does not require simulation or real-world deployment.

### Solution

B.

### Explanations

Eliminates distribution shift between training and inference. This is a fundamental concept in imitation learning and autonomous robotics. The "loop" refers to whether the model's actions affect the next state it observes during the learning process.

In open-loop learning, a model is trained on a fixed dataset of expert demonstrations. The model learns: "In state $s$, do action $a$."

However, at ingerence time, the model will inevitably make a tiny mistake. Because it is in a "closed loop" with the real world, that mistake leads to a slightly different state that what was in the training data. These errors compound quickly.

1. The model makes a small error.
2. It enters a state it never saw during training (distribution shift).
3. Because it hasn't learned what to do in that "weird" state, it makes an even bigger error.
4. The system "drifts" off the path and eventrally crashes.

For choice B, closed-loop learning such as Reinforcement Learning or DAgger allows the model to interact with the environment during training. It exxperiences the consequences of its own actions. It learns how to recover from its own mistakes. By training on the states it actually visits rather than just the expert's perfect states the "training distribution" matches the "inference distribution," effectively eliminating the shift.

For choice A, no need for expert demonstration data. It is incorrect. Many closed-loop methods, like DAgger, specifically rely on an expert to "label" the new states the model visits.

For choice C, lower computational cost during training. It is also incorrect. Closed-loop learning is significantly more expensive because you have to constantly run a simulator or the real robot and potentally query an expert for new labels.

For choice D, does not require simulation or real-world deployment. This is the opposite of the truth. A simulator or a real-world environment is a must to close the loop; otherwise, it may stack with a static (open-loop) dataset.

## Multiple Choice 4

### Problem Description

What is a core challenge of using Reinforcement Learning (RL) for real-world autonomous driving planning?

A. Cannot incorporate expert demonstration data.

B. Prohibitive sample inefficiency and safety risks.

C. Lacks interpretability compared to rule-based methods.

D. Cannot handle dynamic obstacle interactions.

### Solution

B.

### Explanations

While Reinforcement Learning (RL) has shown incredible success in virtual environments, transitioning that success to the "real-world" roads is difficult because of how RL agents learn.

Why Choice B is correct? Reiniforcement Learning is fundatmentally based on trial and error. In an autonomous driving context, this leads two massive roadblocks.

One is sample inefficiency. To learn a complex behavior, like navigating a busy intersection, an RL agent might need millions of training episodes. In the real world, we cannot afford the time of the fleet size required to gather that much data through "discovery".

The other one is safety risks (the exploration problem). For an RL agent to learn that "crashing into a wall is bad," it technically has to "explore" that action or get very close to it to receive a negative reward. In a simulator, this is fine; in a 4000-pound vehicle on a public road, "exploring" a collision is catastrophic. This makes safe exploration one of the biggest hurdles in the field.

Why other choices are incorrect?

For A, cannot incorporate expert demonstration data. This is false. Techniques like Imitation Learning or Pre-training with Expert Demonstrations are frequently used to kickstart an RL agent so it does not start from zero.

For C, lacks interpretaility compared to rule-based methods. While true that RL is a "black box" compared to "if-then" rules, this is a general weakness of all deep learning methods. Choice B is considered a more specific core challenge for deployment because even a "black box" that worked perfectly would be deployed if it weren't so dangerous and slow to train.

For D, cannot handle dynamic obstacle interactions. This is incorrect. One of the main reasons researchers want to use RL is that it is actually better at handling complex, dynamic interactions than rigid, rule-based systems. The problem is not the capability, it is the training process.

The "Sim-to-REal" Gap

To solve these issues, researchers often train RL agents in high-fidelity simulators. However, this introduces the Reality Gap, a policy that learns to drive perfectly in a simulation may fail in the real world because the simulator's physics or sensor noise is not 100% accurate.

## Multiple Choice 5

### Problem Description

In camera-based 3D perception, what is the core idea of the "Lift to 3D at Input" approach?

A. Convert image to pseudo-LiDAR via dense depth prediction.

B. Re-parameterize output of off-the-shelf 2D detectors.

C. Project 2D feature map to 3D/BEV space via camera intrinsic/extrinsic

D. Detect objects in 2D first then localize them in 3D.

### Solution

C.

### Explanations

The correct answer is C. Project 2D feature map to 3D/BEV space via camera intrinsic/extrinsic.

This approach is the foundation of modern Bird's Eye View (BEV) perception stacks, famouly popularized by architectures like LSS (Lift, Splat, Shoot). Instead of detecting objects in individual images and trying to "switch" them together later, the model transforms the raw visual data into a 3D-consistent space early in the pipeline.

The "code idea" is a geometric transformation. Because 2D images lack depth, the model must "lift" the 2D features into 3D. This involves:

1. Feature Extraction: Running a standard backbone on the 2D images.
2. Lifting: Assigning a depth estimate (often a probability distribution) to each feature.
3. Geometric Projection: Using the Camera Intrinsics ($K$) and Extrinsics ($[R \vert t]$) to map those 2D features into a 3D voxel grid of BEV plane. THe math follows the standard pinhole camera projection model:

$$
z = \begin{bmatrix}
    u \\ v \\ 1
\end{bmatrix} = K (R \mathbf{X} + t)
$$

where $\mathbf{X}$ is the 3D coordinate and $(u, v)$ is the image coordinate.

Why the others are incorrect?

For A, convert image to pseudo-LiDAR. THis is specifically the Pseudo-LiDAR paradigm. While it is a form of lifting, it focusses on creating a point cloud from depth maps rather than projecting features maps into a unified latent space. It is considered a subset/precursor to the more general feature-lifting in Choice C.

For B, Re-parameterize output of 2D detectors. This is known as "Lifting at the Output." Models like FCOS3D detect objects in 2D first and then use geometric constraints to "guess" the 3D bounding box dimensions. This is the opposite of Choice C, which lifts at the input/feature stage.

For D, dtect objects in 2D first. This refers to "2D-Driven 3D Detection". It relies on a 2D crop to narrow down the search space in 3D, rather than creating a global 3D representation from the start.

Key Advantage

By lifting to 3D at the input/feature level, the system can naturally fuse data from multiple cameras into a single, seamless top-down view. This eliminates the "double-counting" of objects that appear in the overlap between two camera feeds.

## Multiple Choice 6

### Descriptions

What is a major disadvantage of Model Predictive Control (MPC) for autonomous driving?

A. Cannot handle hard constraints (e.g. max steering rate)

B. Slower than traditional methods (e.g. PID) due to optimization

C. Requires no vehicle model to operate

D. Fails to minimize tracking errors for dynamic trajectories

### Solution

B.

### Explanations

Slower than traditional methods (e.g. PID) due to optimization. Model Predictive Control (MPC) is incredibly powerful, but its "brain" has to work much harder than a PID controller or a simple Pure Pursuit algorithm.

Unlike PID, which simply reacts to the current error, MPC is an online optimizer. At every single time step, the vehicle must:

1. Predict the future state of the car over a "horizon" (e.g. the next 2 - 5 seconds).
2. Solve a complex mathematical optimization problem to find the best sequence of steering and acceleration commands.
3. Repeat this entire process for the next time step.

If you have a complex non-linear vehicle model or a long prediction horizon, this requires significant CPU/GPU power. In the high-spped world of autonomous driving, if the optimization takes too long to solve, the "optimal" command arrives too late to be useful.

Why the others are incorrect?

For A, cannot handle hard constraints. This is actually the main advantage of MPC. It is specifically designed to respect physical limits, like "don't turn the steering wheel faster than 40 degrees/sec" or "don't exceed 65 mph."

For C, requires no vehicle model to operate. This is incorrect. MPC is a model-based controller. It absolutely requires a mathematical model (kinematic or dynamic) to predict how the car will behave in the future.

For D, fails to minimize tracking errors. Incorrect. MPC is generally better at minimizing tracking errors than PID because it "sees" the curves comming in the trajectory and can start turning on the path perfectly.

Summary, the MPC trade-off.

Think of PID as a driver who only looks at the road 1 foot in front of the bumper, fast and simple, but twitchy. Think of MPC as a driver looking 100 feet ahead and calculating the perfect line, smooth and safe, but requires a lot of "mental" effort to process.

## Multiple Choice 7

### Descriptions

What is the key function of the Derivative (D) element in a PID controller?

A. Amplify the current error to speed up response.

B. Correct steady-state errors to speed up response.

C. Introduce damping to reduce overshoot and oscillation.

D. Switch abruptly between two control states.

### Solution

C.

### Explanations

While the Proportional (P) term looks at the present and the Integral (I) term looks at the past, the Derivative (D) term is the "prophet" of the PID controller, it looks at the future.

The Derivative term calculates the rate of change of the error. Its primary job is to act as a "brake" or a shock absorber for the system.

- Anticipation: If the error is decreasing rapidly (meaning the system is approaching the target very fast), the derivative of the error becomes negative. This substracts from the total control output.
- Damping: By pushing back against the motion as the system nears its goal, the D term prevents the system from "sprinting" past the target. This reduces overshoot and settles the oscillations that a high P-gain would otherwise cause.

Why the others are incorrect?

For A, amplify the current error. THis is the job of the Proportional (P) term. A higher P-gain makes the system more aggresive in the present but oftern leads to overshoot.

For B, correct steady-state errors. This is the job of the Integral (I) term. The I term accumulates small errors over time to ensure the system eventually reaches the exact target, even if there is a constant bias (like wind pushing against a drone).

For D, switch abruptly between two states. This describes a Bang-Bang controller (like a basic thermostat that is either 100% ON or 100% OFF), not a PID controller, which provides a smooth, continuous output.

The "D" Trade-off: Noise

While the D term is great for stability, its biggest weakness is high-frequency noise. Sins the derivative is the slope of the error, a tiny bit of "jitter" in a sensor reading can result in a massive, spikey derivative. This is why, in real-world applications, the D-term is almost always paired with a low-pass filter to keep the "brakes" from stuttering.

The standard PID formula in the time domain is:

$$
u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d \tau + K_d \frac{de(t)}{dt}
$$

## Simple Answer Question 1

### Descriptions

Compare the core differences between GNSS-based and Environment Perception-Based localization in update rate and drift characteristics, and analyze the impact of these differences on the real-time control of autonomous driving.

### Explanations

| Feature | GNSS | Environment Perception |
| --- | --- | --- |
| Update Rate | Low (Typically 1Hz - 10Hz) | High (Typically 10Hz - 100Hz) |
| Drift | Zero cumulative drift (Global reference) | Cumulative drift (Incremental relative motion) |
| Accuracy | Decimeters (RTK) to Meters (Standard) | Centimeters (Relative to surroundings) |
| Environment | Needs clear sky (Fails in tunnels/canyons) | Needs features (Fails in fog/featureless roads) |

**GNSS: The "Noisy Truth"**

GNSS provides a global coordinate. Its greatest strength is that it does not "drift." If one is at Point A, and he drive 100 miles to Point B, the error at Point B is roughly the same as it was at Point A.

- The Problem: The update rate is slow (10Hz is standard for high-end units). In a caar moving at 65 mph (approx. 29 m/s), a 10 Hz sensor only gives a position every 2.9 meters. For high-speed control, that is a massive, dangerous gap. Furthermore, GNSS suffers from multipath errors in cities, causing the position to "jump" suddenly by several meters.

**Environment Perception: The "Smooth Liar"**

EPB methods (like LiDAR mapping or Visual Odometry) compare current sensor data to previous frames or a pre-built HD map.

- The Strengh: Because these sensors (Cameras/LiDAR) operate at high frequencies and prvide dense data, the localization is incredibly smooth and precise in the short term.
- The Problem: Because they calculate motion relative to the last know position, tiny errors add up. This is cumulative drift. Without a global reset, a car relying solely on odometry will think it is in a different zip code than it actually is.

**Impact on Real-Time Control**

The differences between these two systems dictate how the vehicle actually "feels" on the road.

**The "Control Latency" Problem**

Vehicle controllers (like the PID or MPC mentioned earlier) usually require feedback at 50Hz to 100Hz to maintain stability, especially at high speeds.

- If used GNSS alone, the controller would be "blind" between those 10Hz updates. The car would likely oscillate or "hunt" for the lane center because the feedback is too slow to catch small deviations.
- EPB and IMUs (Inertial Meaurement Units) provide the high-frequency "filter" that allows the controller to make micro-adjustments to steering and braking every few milliseconds.

**Glabal vs. Local Consistency**

- Real-Time Steering: This relies on Local Consistency. The car doesn't care if its Latitude/Longitude is off by a meter as long as it knows it is exactly 20cm from the left lane line. Perception-based localization excels here.
- Path Planning: This relies on Global Consistency. if the car is navigating to a specific highway exit 5 miles away, it needs GNSS to ensure it does not drift off the global map and miss the turn entirely.

## Simple Answer Question 2

### Descriptions

Explain the three main approaches of "Lift to 3D" in camera-based 3D perception (Output/Input/Feature Map), and list one strength for each approach.

### Explanations

In camera-based 3D perception, "lifting" refers to the mathematical and architectural process of recovering the missing depth dimension to transform 2D image data into a 3D spatial representation. The three approaches differ primarily in where in the neural network pipeline this transformation occurs.

**1. Lift at Output (Image-to-3D)**

In this approach, the network processes the image entirely in 2D using standard convolutional backbones. The "lifting" happens at the very end (the prediction heads), where the model attempts to regress 3D properties directly from 2D features.

- How it works: A 2D detector identifies an object and its bounding box. The model then predcts additional 3D attributes like the object's depth ($d$), 3D dimensions $(w, h, l)$, and orientation ($\theta$) based on visual cues (e.g. the object's size in the image or perspective lines).
- Key Strength: Low Computational Latency. Since the heavy lifting (feature extraction) happens in 2D, these models are extremely fast and can often run in real-time on low-power hardware.

**2. Lift at Input (Pseudo-LiDAR)**

This approach transforms the 2D data into a 3D representation before the main detection of perception task begins. It essentially tries to make a camera "act" like a LiDAR sensor.

- How it works: The system uses a dedicated sub-network to perform dense depth estimation for every pixel in the image. This depth map is then projected into 3D space using camera intrinsics to create a Pseudo-LiDAR point cloud. A standard 3D detection network (designed for LiDAR) is then run on this point cloud.
- Key Strength: High Interpretability. Because the intermediate output is a physical 3D point cloud, engineers can visually inspect the depth accuracy, making it easier to debug sensor calibration or depth estimation errors.

**3. Lift at Feature Map (BEV/Latent Space)**

This is the state-of-the-art approach used by systems like Tesla's FSD and various BEV (Bird's Eye View) models. It lifts intermediate latent features rather than raw pixels or final detections.

- How it works: A 2D backbone extracts feature maps from multiple camera images. These feature maps are then projected into a unified 3D "Voxel" or 2D "BEV" grid using geometric transformations (like the Lift-Splat-Shoot method). THe network then performs detection or motion planning directly in this unified top-down space.
- Key Strenght: native Multi-View Fusion. Because the features are lifted into a common 3D space, the model can seamlessly "switch" together information from different cameras. An object partially visible in the front-left and side-left cameras is seen as a single coherent entity in the BEV feature map.

**Summary Table**

| Approach | Where Lifting Occurs | Core Mechanism | Main Strength |
| --- | --- | --- | --- |
| Output | Prediction Heads | Regression of 3D attributes | Speed & Efficiency |
| Input | Pre-processing | Dense Depth $\rightarrow$ Point Cloud | Physical Interpretability |
| Feature Map | intermediate Layers | Geometric Projection to BEV | Seamless Multi-Camera Fusion |

## Simple Answer Question 3

### Descriptions

What are affordances in motion planning, and what are their key advantages and limitations compared to direct actuation output (steering/acceleration)?

### Explanations

In the context of motion planning, affordances are intermediate, high-level features that represent the "state of the world" in a way that is directly relevant to driving tasks.

Instead of a model trying to go straight from raw camera pixels to a steering wheel angle (End-to-End) or trying to identify every single pixel in the scene (Semantic Segmentation), it extracts specific "labels" that matter for decision-making.

What do they look like?

Typical affordances in autonomous driving include:

- Distance to the lane center: How far left or right am I?
- Heading angle: Is my car pointed parallel to the road or at an angle?
- Distance to the preceding vehicle: How much "gap" do I have?
- Relative speed: is the car in front pulling away or braking?

**Advantages Over Direct Actuation (End-to-End)**

The "Direct Acuation" approach, where a neural network takes an image and outputs a torque value, is often called a black box. Affordances offer a more transparent middle ground.

- Interpretability: If the car suddenly swerves, we can check the affordances. Did it think the lane center was 2 meters to the left? This makes debugging and safety validation much easier.
- Reduced Distribution Shigt: Steering commands are specific to a single car's physics (tires, weight, steering rack). Affordances (like "I am 1 meter from the curb") are universal. A model trained to find affordances on a Toyota can theoretically be used on a Tesla without starting from scratch.
- Easier Learning Task: It is mathematically "easier" for a network to learn geometry (lines and distances) than it is to learn the complex mapping between a blurry pixel and the exact voltage needed for a steering motor.

**Limitations of Affordances**

While they sound like the perfect compromise, affordances introduce their own set of "robotic" headaches.

- Information Bottlenecks: By condensing a rich, 4K camera feed into just 10 or 15 numbers (affordances), you might throw away the "reason" for an action. For example, if there is a massive pothole in the lane, a standard affordance model might not have a "distance to pothole" label, forcing the car to drive right through it because it only cares about "distance to lane center."

- Cascading Errors: If the affordance extractor makes a mistake (e.g., misidentifying a guardrail as a lane line), the motion planner will perfectly execute a "safe" path right into the rail. In direct actuation, the model might implicitly learn to avoid the visual texture of a guardrail.

- Manual Engineering: Someone has to decide which affordances matter. If you forget to include "pedestrian intent" as an affordance, your planner will be "blind" to the person about to step off the curb until they are physically in the way.

**Comparison Summary**

| Feature | Direct Actuation (End-to-End) | Affordance-Based Planning |
| --- | --- | --- |
| Logic | "See pixel, move wheel" | "See world, calculate distances, move wheel" |
| Transparency | Low (Black Box) | High (Explainable) |
| Generalization | Specific to vehicle dynamics | Generic across different vehicles |
| Failure Mode | Unpredictable "weird" behaviors | Predictable but limited by labels |

## Simple Answer Question 4

### Descriptions

Given the pseudocode of the traditional CV lane detection algorithm, answer the following questions:

(1) What is the purpose of the ROI masking step?
(2) Which line converts edge pixels into detected lane segments?

Algorithm 1 CV Lane Detection

Input: RGB Image $I \in \mathbb{R}^{H \times W \times 3}$

Parameters: Gaussian kernel $k$, Canny thresholds $T$, ROI vertices $V$, Hough parameters $\rho$

Output: Set of detected lane segments $\mathcal{L} = \{(x_1, y_1, x_2, y_2)\}$

1 $G \leftarrow \text{ConvertToGrayscale}(I)$

2 $B \leftarrow \text{GaussianBlur}(G, k)$

3 $E \leftarrow \text{CannyEdgeDetection}(B, T)$

4 $M_{roi} \leftarrow \text{GenerateMask}(V, shape(E))$

5 $E_{masked} \leftarrow E \bigodot M_{roi}$

6 $\mathcal{L} \leftarrow \text{HoughLineTransform}(E_{masked}, \rho)$

7 $\text{return } \mathcal{L}$

### Explanations

(1) Purpose of the ROI Masking Step

Th Region of Interest (ROI) masking step is designed to eliminate visual noise and irrelecant data from the image.

In a standard driving scene, the top half of the frame typically contains the sky, trees, or buildings. While these areas may contain "edges" (captured by the Canny detector in Line 3), they are not relevant to lane tracking. By applying the mask $M_{roi}$ via element-wise multiplication ($\bigodot$).

- Accuracy: It prevents the Hough Transform from accidentally detecting the horizon, power lines, or guardrails as lanes.
- Efficiency: It restricts the search space for the line dtection algorithm to the specific portion of the road where lanes are physically expected to appear.

(2) Conversion of Edge Pixels into Lane Segments

The line that performs this conversion is Line 6:

$$
\mathcal{L} \leftarrow \text{HoughLineTransform}(E_{masked}, \rho)
$$

Until this point in the algorithm, the data consists of a "mask" or a "map" of pixels, $E_{masked}$ is essentially a binary image where $1$ is an edge and $0$ is nothing. The computer doesn't "know" these are lines yet, it just sees a collection of scattered points.

The Hough Line Transform is the mathematical bridge. It uses a voting procedure in a parameter space (Hough Space) to determine which sets of edge pixels align into a coherent linear shape. It then outputs the coordinates ($\mathcal{L}$) of the start and end points of those segments, effectively turning "dots on a grid" into "vector geometry".
