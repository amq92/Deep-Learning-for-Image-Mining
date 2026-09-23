---
layout: section
number: 04
---

::title::

# Transferable *visual representations*

::question::

How can features learned for one task become useful for another?

<!--
**Purpose:** Shift from examining representations inside their original model to reusing them under new learning conditions

**Say:** The next step is not to discard task-dependent learning. It is to use one learned encoder as the starting point for several downstream problems

**Transition:** First compare the task-specific and transfer workflows directly
-->
---
layout: lesson
---

::title::

# From one model per task to a *reusable representation*

::text::

- **Training from scratch repeats the investment**  
  Each new task needs labeled data, compute, and optimization

- **Transfer separates learning into two stages**  
  A large source dataset trains an encoder once

- **Downstream tasks reuse that encoder**  
  Smaller target datasets adapt it instead of learning every feature from zero

- **The encoder is a starting point, not a guarantee**  
  Its usefulness depends on the downstream task and data

> One large learning investment can support many downstream tasks

::visual::

```mermaid {theme: 'neutral', scale: 0.50}
%%{init: {"themeVariables": {"fontSize": "14px"}, "flowchart": {"nodeSpacing": 18, "rankSpacing": 16, "diagramPadding": 6, "subGraphTitleMargin": {"top": 12, "bottom": 12}}}}%%
flowchart TB
  subgraph specific["Task-specific learning"]
    direction LR
    A1("Task A<br/>data") --> M1("Model A")
    A2("Task B<br/>data") --> M2("Model B")
  end

  subgraph transfer["Transfer learning"]
    direction TB
    P("Large source dataset<br/>+ source task") --> E("Pretrained encoder")
    E --> D1("Downstream<br/>task A")
    E --> D2("Downstream<br/>task B")
  end

  specific --> transfer

  style P fill:#fff2cc,color:#7c5b00,stroke:#d6a900,stroke-width:2px
  style E fill:#df2439,color:#fff,stroke:#df2439,stroke-width:2px
  linkStyle 3,4 stroke:#df2439,stroke-width:2px
  linkStyle 5 stroke:transparent,fill:none
```

<!--
**Purpose:** Establish transfer as a change in the learning workflow, not a claim that representations are independent of tasks

**Say:** A model developer performs pretraining by optimizing an encoder for a source task. The result still reflects that source data and objective; it is a learned starting point, not a representation that transfers to everything

**Preview:** Transfer depends on how well the source representation matches the downstream task and domain. We will examine that after the reuse strategies

**Transition:** ImageNet classification became the influential supervised example of how pretraining is performed
-->
---
layout: lesson
ratio: balanced
---

::title::

# The head is task-specific, the *encoder can transfer*

::text::

- **Source task:**  
  Train a classifier on **ImageNet-1K** : about 1.2M training images and 1,000 classes

- **Two model parts:**  
  The encoder learns the representation; the head predicts the ImageNet classes

- **Transfer:**  
  Reuse the pretrained encoder with a custom task-specific head

- **Common pretrained models:**  
  AlexNet, VGG, ResNet, DenseNet

::visual::

```mermaid {theme: 'neutral', scale: 0.82}
%%{init: {"themeVariables": {"fontSize": "15px"}, "flowchart": {"nodeSpacing": 16, "rankSpacing": 18, "diagramPadding": 20, "subGraphTitleMargin": {"top": 12, "bottom": 10}}}}%%
flowchart LR
  subgraph pretrain["1 · Pretrain on ImageNet-1K"]
    direction TB
    I("ImageNet<br/>images") --> E1("Visual encoder") --> H1("ImageNet head") --> Y1("1,000 classes")
  end

  subgraph transfer["2 · Transfer to a new task"]
    direction TB
    N("New-task<br/>images") --> E2("Visual encoder") --> H2("New task head") --> Y2("New predictions")
  end

  pretrain --> transfer

  style E1 fill:#df2439,color:#fff,stroke:#df2439,stroke-width:2px
  style E2 fill:#df2439,color:#fff,stroke:#df2439,stroke-width:2px
  style H1 fill:#f3f4f6,color:#6b7280,stroke:#9ca3af,stroke-dasharray:4 3
  style H2 fill:#fff2cc,color:#7c5b00,stroke:#d6a900,stroke-width:2px
  linkStyle 6 stroke:transparent,fill:none
```

<div class="source-note">Deng et al., 2009 · Krizhevsky et al., 2012 · Simonyan & Zisserman, 2015 · He et al., 2016 · Huang et al., 2017 · Razavian et al., 2014</div>

<!--
**Purpose:** Separate the source-specific classifier from the representation that may be reused

**Say:** ImageNet pretraining performs one task: classification. The head maps features to its 1,000 classes; transfer keeps the encoder and replaces that head

**Examples:** AlexNet, VGG, ResNet, and DenseNet are familiar architectures commonly distributed with ImageNet-1K pretrained weights

**Evidence:** Deng et al. introduced ImageNet; Krizhevsky et al., Simonyan and Zisserman, He et al., and Huang et al. introduced the listed architectures through ImageNet classification; Razavian et al. demonstrated downstream reuse of ImageNet-trained CNN features

**Boundary:** Larger web-scale and self-supervised sources exist; introduce them later with their different data and objectives

**Transition:** Next, keep this encoder fixed and examine what its existing features can already support
-->
---
layout: lesson
ratio: balanced
---

::title::

# Transfer strategies differ by *how much we adapt*

::text::

- **Linear probing**  
    Train a linear head; keep the encoder fixed. Helps evaluate what is linearly accessible.

- **Frozen feature extraction**  
    Train a richer, nonlinear head; keep the encoder fixed.

- **Partial fine-tuning**  
    Update the new head and selected encoder layers.

- **Full fine-tuning**  
    Update the entire model from pretrained—not random—weights.

> All strategies start from pretrained weights; they differ in how much may adapt.

::visual::

<div class="transfer-strategies">
  <div class="strategy-stage">
    <div class="strategy-label"><strong>1 · Linear probe</strong><span>replace the source head</span></div>
    <div class="network-blocks">
      <div class="net-block frozen wide">Early encoder</div>
      <div class="net-block frozen medium">Middle</div>
      <div class="net-block frozen narrow">Upper</div>
      <div class="net-arrow">→</div>
      <div class="net-block trainable head">Linear head</div>
    </div>
  </div>

  <div class="strategy-stage">
    <div class="strategy-label"><strong>2 · Partial fine-tuning</strong><span>unfreeze selected layers</span></div>
    <div class="network-blocks">
      <div class="net-block frozen wide">Early encoder</div>
      <div class="net-block frozen medium">Middle</div>
      <div class="net-block trainable narrow">Upper</div>
      <div class="net-arrow">→</div>
      <div class="net-block trainable head">New head</div>
    </div>
  </div>

  <div class="strategy-stage">
    <div class="strategy-label"><strong>3 · Full fine-tuning</strong><span>unfreeze the complete model</span></div>
    <div class="network-blocks">
      <div class="net-block trainable wide">Early encoder</div>
      <div class="net-block trainable medium">Middle</div>
      <div class="net-block trainable narrow">Upper</div>
      <div class="net-arrow">→</div>
      <div class="net-block trainable head">New head</div>
    </div>
  </div>

  <div class="strategy-legend">
    <span><i class="legend-swatch frozen"></i>Frozen</span>
    <span><i class="legend-swatch trainable"></i>Trainable</span>
  </div>
</div>

<style>
.transfer-strategies { display: flex; flex-direction: column; gap: 0.65rem; width: 100%; }
.strategy-stage { padding: 0.65rem 0.7rem; border: 1px solid #d1d5db; border-radius: 0.55rem; background: #fff; }
.strategy-label { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 0.5rem; color: #1f2937; }
.strategy-label strong { font-size: 0.86rem; }
.strategy-label span { font-size: 0.65rem; color: #6b7280; }
.network-blocks { display: flex; align-items: center; gap: 0.28rem; }
.net-block { display: flex; align-items: center; justify-content: center; height: 2.15rem; border-radius: 0.3rem; font-size: 0.61rem; line-height: 1.05; text-align: center; font-weight: 600; padding: 0 0.25rem; }
.net-block.wide { flex: 1.25; }
.net-block.medium { flex: 0.9; }
.net-block.narrow { flex: 0.7; }
.net-block.head { flex: 0.85; }
.net-block.frozen, .legend-swatch.frozen { background: #e5e7eb; color: #374151; border: 1px solid #9ca3af; }
.net-block.trainable, .legend-swatch.trainable { background: #df2439; color: #fff; border: 1px solid #df2439; }
.net-arrow { color: #6b7280; font-size: 0.9rem; }
.strategy-legend { display: flex; justify-content: center; gap: 1.2rem; color: #4b5563; font-size: 0.67rem; }
.strategy-legend span { display: flex; align-items: center; gap: 0.3rem; }
.legend-swatch { display: inline-block; width: 0.75rem; height: 0.75rem; border-radius: 0.18rem; }
</style>

<!--
**Purpose:** Present transfer as a choice about which parameters may learn

**Distinguish:** Replacing the source head defines the downstream predictor; freezing defines which retained layers can change

**Interpret:** A richer head can compensate for a less directly organized feature space, so its accuracy is less diagnostic of representation quality

**Decision cue:** Prefer the least adaptation that reaches the required downstream performance

**Transition:** The best strategy depends on whether source and downstream data require similar visual distinctions
-->
---
layout: comparison
---

::title::

# Transfer depends on *task and domain alignment*

::left::

### 🎯 Task shift

The prediction goal changes

- **Classification → Detection**  
    Name what is present *vs.* locate each object
- **Classification → Segmentation**  
    Label the image *vs.* label every pixel
- **Coarse → Fine classification**  
    Animal category *vs.* individual species

::right::

### 🖼️ Domain shift

The downstream images differ from the pretraining images

- **Content**  
    Everyday objects *vs.* medical tissue
- **Modality**  
    RGB images *vs.* gray-level X-ray images
- **Capture conditions**  
    Frontal *vs.* aerial viewpoint

::takeaway::

> Either mismatch can weaken transfer and require more adaptation.

<!--
**Why it matters:** One encoder may transfer well to one problem and poorly to another.

**Ask:** Did the goal change, the images change, or both?

**Caution:** Visual resemblance does not guarantee statistical alignment.

**Transition:** Source accuracy is not enough; next, we choose how to adapt based on transfer success.
-->
---
layout: lesson
---

::title::

# Transfer works when *representations align*

::text::

- **Why transfer works**  
Pretraining learns generic visual patterns: edges, textures, shapes, objects.  
Encoder discovers hierarchical structure that applies across many vision tasks.

- **How to transfer**  
Different adaptation levels are possible:  
Linear probe → nonlinear head → partial fine-tuning → full fine-tuning.  
Choose based on transfer strength.

- **When transfer works**  
Task shift and domain shift determine success.  
Misalignment weakens transfer. Strong alignment enables reuse.

> A good visual representation can be reused beyond the task used to learn it.




<!--
**Why?** Generic features from pretraining → **When?** Task/domain alignment → **How?** Adapt based on transfer strength.

**One idea:** Value is conditional, not universal.

**Pause here:** Let students sit with the tension: pretrained is powerful *and* limited. Both true.

**Transition:** Next: a new encoder architecture (Transformers) that changes what gets preserved and how far it transfers.
-->
