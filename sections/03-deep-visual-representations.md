---
layout: section
number: 03
---

::title::

# Deep *visual representations*

::question::

What is learned inside a deep network?

<!--
**Purpose:** Move from general learning mechanics to representations inside a visual encoder

**Transition:** First refresh the operation at the core of a convolutional encoder
-->
---
layout: lesson
ratio: visual
---

::title::

# How convolution builds a *feature map*

::text::

- A convolution operator slides a **kernel** (a small matrix of weights) across the input
- At each position, element-wise products are summed into **one output value**
- The **output feature-map size** is:

$$H_{out}=\left\lfloor\frac{H+2p-k}{s}\right\rfloor+1$$

<div class="conv-parameters">
  <div><span><i>H</i></span><b>input size</b><code>5</code></div>
  <div><span><i>k</i></span><b>kernel size</b><code>3</code></div>
  <div><span><i>s</i></span><b>stride</b><code>1</code></div>
  <div><span><i>p</i></span><b>padding</b><code>0</code></div>
  <div class="result"><span><i>H</i><sub>out</sub></span><b>output size</b><code>3</code></div>
</div>

<style>
.conv-parameters { display: grid; gap: 0.22rem; margin: 0.45rem auto 0; max-width: 12.5rem; }
.conv-parameters > div { align-items: center; display: grid; gap: 0.4rem; grid-template-columns: 2.5rem 1fr 1.5rem; }
.conv-parameters span { background: var(--surface-2); border: 1px solid var(--line); border-radius: 999px; color: var(--accent); font-family: 'Times New Roman', serif; font-size: 0.68rem; font-weight: 800; padding: 0.08rem 0.3rem; text-align: center; }
.conv-parameters sub { font-size: 0.62em; }
.conv-parameters b { color: var(--muted); font-size: 0.6rem; font-weight: 600; }
.conv-parameters code { background: transparent; color: var(--text); font-size: 0.66rem; font-weight: 800; text-align: right; }
.conv-parameters .result { border-top: 1px solid var(--line); margin-top: 0.1rem; padding-top: 0.28rem; }
</style>


::visual::

<ConvolutionOperationVisual />

<!--
**Purpose:** Introduce the operator gently before connecting its arithmetic to output dimensions

**Start:** First trace the direction of the diagram without discussing notation: input patch, shared kernel, one output value. Then say: “the convolution repeats this small weighted-sum operation across the image”

**Animation 1:** Advance once. Name this movement as stride 1, and emphasize that the same kernel values are reused

**Animation 2:** Advance again. One scalar per valid position forms one feature map

**Output size:** Use the numbers beneath the formula: input 5, kernel 3, no padding, stride 1 gives output 3. Do not derive the general formula unless asked

**Channels:** The matrix suppresses the channel dimension for clarity. With an RGB input, one kernel spans all three input channels; using several kernels creates several output channels

**Go deeper only if needed:** In CNN libraries this is usually cross-correlation because the kernel is not flipped; the name “convolution” is conventional. Bias and nonlinearity come after this weighted sum

**Transition:** The procedure is identical for every kernel; what changes is the image pattern to which it responds
-->
---
layout: lesson
ratio: visual
---

::title::

# Kernels turn pixels into *feature maps*

::text::

- Different kernels extract **different visual features**
- Each kernel spans all input channels and produces **one feature map**
- A strong activation marks **where its feature is present**
- A bank of $K$ kernels creates $K$ maps, stacked as **output channels**

> The key shift: kernel weights are learned, not handcrafted

::visual::

<ConvolutionFilterExamples />

<!--
**Key message:** Convolution changes the representation: pixels become maps of detected visual features

**Point to the outputs:** These are not transformed pictures to interpret as images. Each location stores the strength of one feature

**Top row:** Use blur only to establish that changing the local weighting changes what information survives

**Bottom row:** The image is smoothed before the Sobel kernel measures horizontal intensity change, reducing noise while preserving the main vertical boundaries. In the grayscale output, mid-gray is zero, black is negative, and white is positive

**RGB row:** Read the three colored slices as one kernel spanning all input channels. Before ReLU, mid-gray means no color preference; lighter areas favor warm colors and darker areas favor blue

**Animation:** Advance once. Only the two signed outputs change. In the lower map, the blue evidence is removed, leaving the yellow body and red beak bright

**Connect to the chapter:** These recognizable filters bridge the two paradigms. Earlier representations used kernel values chosen by experts; learned representations learn those kernel weights from data

**ReLU:** It is applied after convolution and keeps one sign of the response; it does not define a new detector

**Blur caveat:** The displayed $3\times3$ Gaussian matrix illustrates low-pass weighting; the output uses stronger smoothing so the effect remains visible at slide scale

**Transition:** Now ask why applying learned local filters everywhere is especially suitable for images
-->
---
layout: lesson
ratio: balanced
---

::title::

# Why does convolution work well for *images?*

::text::

- **Local connectivity**  
  Respects spatial neighborhoods
- **Weight sharing**  
  Searches for the same pattern at every position
- **Reduced parameter count**  
  Depends on the kernel, not the image dimensions
- **Practical depth**  
  Fewer parameters make stacking many layers feasible

> The architecture imposes these constraints. Learning determines the kernel weights

::visual::

<ImageStructureVisual />

<!--
**Purpose:** Introduce convolution as an inductive bias: a useful assumption built into the architecture before learning begins

**Start with the contrast:** Trace the upper path. An MLP can learn from flattened pixels, but it receives no built-in notion of two-dimensional neighborhoods

**Name the two assumptions:** Convolution assumes that local interactions matter and that the same detector can be useful at different positions

**Separate bias from learning:** We choose locality and sharing. Training still discovers the filter values that help the task

**Compare like with like:** Both paths produce one 224 × 224 output map. Dense connectivity needs one independent weight from every input value to every output value: 150,528 × 50,176 = 7,552,892,928 weights

**Convolution count:** One 3 × 3 kernel spanning RGB has 3 × 3 × 3 = 27 weights. Reusing those same weights at all 50,176 positions creates the full map

**Why it matters:** Compare what the model can afford with the same parameter budget. The efficiency leaves room for more transformations and richer feature hierarchies

**Depth bridge:** “Deep” does not follow automatically from convolution, but convolution makes many successive spatial layers practical

**Transition:** The next slide shows what those successive layers gain: a wider view of the input
-->
---
layout: lesson
ratio: balanced
---

::title::

# How encoders reshape *spatial representations*

::text::

- **Receptive field:**  
  Region of the input that can influence an activation at a given layer.
- **Pooling:**  
  Operation that *summarizes* a local spatial neighborhood independently within each channel.

- **Stacked convolution and pooling operations:**   
  Pooling or strided operations reduce $H\times W$, while using more kernels often increases $D$


> Fewer spatial locations, but more context (and often more features) at each location.

::visual::

<ReceptiveFieldGrowth />

<!--
**Purpose:** Give students the precise concept that explains how local operations acquire global context

**Read left to right:** Follow one response through successive stages. Point to the growing highlighted input region rather than reading the bullets

**Concrete example:** Two stacked $3\times3$ convolutions with stride 1 have a $5\times5$ receptive field; three have $7\times7$

**Key distinction:** Ordinary pooling summarizes local neighborhoods and leaves a smaller spatial grid. Only global pooling spans every spatial position and returns one value per channel

**Information trade-off:** Point to the shrinking grids: resolution is traded for wider context and some local robustness. Ask what disappears at the final global step

**Do not overclaim:** “Edges to parts to objects” is a useful tendency, not a fixed dictionary. Channel meanings are distributed and vary across models

**Transition:** Point to the wider context at deeper stages. The next slide shows what kinds of visual evidence those stages can combine
-->
---
layout: lesson
ratio: balanced
---

::title::

# Depth builds increasingly *complex features*

::text::


- **Early layers:**  
  Simple local patterns such as edges and colors
- **Intermediate layers:**  
  Textures, repeated patterns, and shapes
- **Deep layers:**  
  Parts, objects, or task-specific configurations

- **Visualization example** from an AlexNet-like 8-layer CNN
  + **Forward pass:** identify the strongest activations
  + **Left:** project each activation back to the pixels
  + **Right:** show the corresponding source-image patch



::visual::

<FeatureHierarchyEvidence />

<!--
**Goal:** Use the examples as evidence of composition across depth—not as a dictionary of fixed channel meanings

**Model:** Zeiler and Fergus trained an AlexNet-like, eight-layer classifier on 1.3 million ImageNet 2012 images across 1000 classes. It used five convolutional layers, two 4096-unit fully connected layers, and a softmax classifier

**Training details if asked:** Supervised cross-entropy with SGD; batch 128; initial learning rate 0.01; momentum 0.9; dropout 0.5 in layers 6–7; 70 epochs; crops and horizontal flips

**Why it differs from AlexNet:** Its first convolution uses 96 filters of size 7 × 7 with stride 2. The smaller kernel and stride were chosen after the authors found aliasing and missing mid-frequency information with AlexNet’s 11 × 11, stride-4 design

**How Layers 2–5 were produced:** Run the trained CNN over ImageNet validation images. For a selected feature map, retain each of its nine strongest spatial activations and zero the others. Project that activation to pixels through an attached deconvolutional network

**Backward projection:** Repeatedly unpool using max locations saved during the forward pass, rectify with ReLU, then apply transposed—spatially flipped—versions of the learned filters. The deconvolutional network is a diagnostic probe; it is not trained separately

**Read one pair:** The left reconstruction isolates image structure contributing to the response. The right patch is the corresponding receptive-field region from the real validation image. These are not generated samples or raw feature maps

**Start:** Pick one column. Its nine examples come from one feature map, not from different learned filters

**Click:** Layers 3–5 replace Layers 1–2. Ask what becomes recognizable as receptive fields widen and simpler evidence is composed

**Nuance:** Strong examples reveal a response tendency, not everything a channel represents. A channel may respond to related appearances under deformation

**Transition:** These features can be kept spatially organized or summarized, depending on what the task needs
-->
---
layout: lesson
ratio: balanced
---

::title::

# The task determines *which representation to keep*

::text::

- **ResNet · classification**
  - Global pooling produces one compact image vector
  - Keeps **what** is present; discards explicit **where**

- **U-Net · segmentation**
  - A decoder transforms spatial feature maps into a pixel-level output
  - Skip connections restore fine details from the encoder

> A decoder cannot recover precise location from a fully global vector

::visual::

<DenseGlobalNetworks />

<!--
**Purpose:** Contrast two real CNN designs through the information their tasks require

**Start — ResNet:** Follow the upper path. Classification needs one decision for the whole image, so global pooling intentionally removes the spatial grid while retaining a rich channel vector

**Then — U-Net:** Follow the lower path. Segmentation needs a decision at every pixel, so its bottleneck remains spatial. The decoder upsamples these coarse maps

**Critical nuance:** The decoder does not reverse information loss. U-Net's skip connections copy higher-resolution encoder features across the bottleneck, giving the decoder boundaries and fine detail that downsampling blurred

**Compression:** U-Net reduces spatial resolution, but it does not collapse the representation to one global vector. It trades fine resolution for context, then combines coarse semantic features with preserved local evidence

**Other tasks:** Object detectors such as YOLO and Mask R-CNN also retain spatial feature maps. Mask R-CNN adds a mask-prediction branch, but those details are not needed for today's distinction

**Check:** Ask: “If I shuffle the spatial cells before the output head, which task fails most directly?” Segmentation, because location is part of the answer

**Transition:** The useful representation is not the most compressed one; it is the one that keeps the differences required by its task
-->
---
layout: lesson
ratio: balanced
---

::title::

# Learning makes representations *selective*

::text::

- Layers learn to combine **local evidence** into **complex features**
- Training shapes what information is **combined**, **preserved**, or **discarded**
- The task may require **spatial detail** or a **global summary**

- **A learned representation is not neutral:** what it preserves depends on how it was learned

> Which learned features remain useful when the task or domain changes?

::visual::

<ChoiceTriangle />

<!--
**Purpose:** Close with what is distinctive about learned deep representations and open the transfer question

**Read the causal chain:** Architecture constrains the transformations the model can express. Data supplies the visual variations it observes. The objective rewards keeping some distinctions and ignoring others

**Abstraction:** Recall Slide 18. Successive layers express content through combinations of visual evidence that are less tied to exact pixels. This tendency is learned rather than explicitly specified feature by feature

**Invariance and discrimination:** Training can make a representation less sensitive to task-irrelevant variation while preserving distinctions rewarded by the objective. These properties are not unique to neural networks; what changes here is that they emerge jointly from learned transformations across layers

**Do not overclaim:** The network does not consciously decide what matters, and learned representations are not guaranteed to retain everything useful outside the training task

**Transition:** Read the takeaway question, then stop. The next section answers it by separating frozen feature extraction, probing, and adaptation
-->
