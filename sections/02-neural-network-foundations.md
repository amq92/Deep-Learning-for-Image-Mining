---
layout: section
number: 02
---

::title::

# Neural network *foundations*

::question::

How does a neural network learn from examples?

<!--
**Purpose:** Establish the mechanics needed to explain how deep models learn

**If familiar:** Move directly to Section 3

**Transition:** Start with the computation performed by one neuron
-->
---
layout: lesson
ratio: balanced
---

::title::

# From a *perceptron* to a neuron

::text::

- Inputs are combined through a **weighted sum and bias**
- A perceptron uses a **hard threshold** for a binary decision
- Modern neurons use activations suited to **gradient-based learning**

<div class="task-example">
  <strong>Parameters and intermediate values</strong>
  <span><b>Learned parameters:</b> weights <i>w</i> and bias <i>b</i></span>
  <span><b>Computed per input:</b> pre-activation <i>z</i> and activation <i>a</i></span>
</div>

> A neuron converts weighted evidence into an activation

::visual::

<NeuralFoundationsVisual kind="neuron" />

<!--
**Purpose:** Name the computation shared by perceptrons and modern artificial neurons

**Say:** Parameters are the weights and bias. The activation is the value produced for this particular input

**If needed:** A positive weight supports the response; a negative weight opposes it. The bias shifts when the unit responds

**Transition:** One neuron remains limited. Combine many neurons and add nonlinear activations
-->
---
layout: lesson
ratio: balanced
---

::title::

# Nonlinear activations make networks *expressive*

::text::

- A **multilayer perceptron (MLP)** is a feedforward network of fully connected layers
- Each layer applies a learned **affine transformation**, followed by a nonlinear activation
- Without nonlinear activations, stacked layers collapse into **one linear map**
- **ReLU** is a common activation function in hidden layers
- **Sigmoid or softmax** can turn output scores into probabilities

> An MLP composes learned transformations with nonlinear activations

::visual::

<NeuralFoundationsVisual kind="mlp" />

<!--
**Purpose:** Explain the MLP forward pass and the role of activation functions together

**Say:** Read the diagram from left to right once. Every hidden unit receives the previous layer, then applies its activation

**If needed:** ReLU keeps positive values and clips negative ones

**Sigmoid vs. softmax:**
- **Sigmoid:** use for a binary output or independent labels; each output gets its own probability
- **Softmax:** use for mutually exclusive classes; probabilities compete and sum to 1

**Transition:** A forward pass produces a prediction; learning needs a way to judge and correct it
-->
---
layout: lesson
ratio: balanced
---

::title::

# Error drives *parameter updates*

::text::

- **Forward pass**<br>
  Predicts $\hat{y} = f_\theta(x)$
- **Loss function**<br>
  Compares the prediction with the target $L(\hat{y}, y)$
- **Backpropagation**<br>
  Computes the gradients $g = \nabla_\theta L$
- **Optimizer**<br>
  Updates the parameters: $\theta \leftarrow \theta - \eta g$
- **Iterative**<br>
  Repeat learning until convergence

> **The loss function defines what the network learns to optimize**

::visual::

<NeuralFoundationsVisual kind="training" />

<!--
**Purpose:** Complete the learning loop without treating training as one opaque operation

**Say:** For regression, mean squared error is common. For classification, cross-entropy penalizes low probability on the target class

**Visual:** These surfaces are 2D slices through parameter spaces with millions of dimensions. They are maps for comparison—not the complete landscape

**If needed:** Backpropagation computes gradients; it does not change the weights. The optimizer performs the update using those gradients

**Transition:** Training explains how one learned transformation improves. Depth comes from composing many of them
-->
---
layout: lesson
ratio: balanced
---

::title::

# Going deeper creates *new challenges*

::text::

- **Deep** means composing many successive learned transformations
- Stacking more layers increases **representational capacity**
- Depth also makes **optimization and information flow** more difficult
- Architectural and training advances help overcome these difficulties

> Depth creates potential, not a guarantee

::visual::

<NeuralFoundationsVisual kind="deep" />

<!--
**Purpose:** Define “deep,” surface the difficulties introduced by depth, and use residual learning as one influential solution—not the only one

**Other enablers:** Better initialization, normalization, activation functions, optimizers, and careful architecture design also made deep networks practical

**Click:** First explain the identity shortcut as one concrete example. Then reveal the experimental evidence

**Read the figure:** Thin curves are training error; thick curves are validation error. Compare 18 versus 34 layers on each panel—not plain versus residual at one instant

**Why it matters:** The left panel rules out ordinary overfitting: even training error worsens when the plain network is deeper. The right panel shows that changing the architecture lets depth become useful

**Evidence beyond the plot:** The same residual idea enabled a 152-layer network that won the ILSVRC 2015 classification task

**If needed:** A shortcut does not skip learning. It gives each block an easy identity baseline, so the learned branch can focus on the change that is needed

**Source:** He et al., “Deep Residual Learning for Image Recognition,” CVPR 2016

**Transition:** We now know how deep networks can be trained. Next, inspect the visual representations those transformations produce
-->
