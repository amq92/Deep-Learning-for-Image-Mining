---
layout: section
number: 05
---

::title::

# From convolutions to *visual transformers*

::question::

What does self-attention bring to visual representation learning?

<!--
**Purpose:** Move from reusing CNN representations to asking how a different encoder architecture changes the interactions a representation can capture

**Say:** CNNs build visual structure through repeated local operations. Vision Transformers begin from image patches and allow those patches to interact globally

**Transition:** First identify the architectural assumption that makes convolution effective for images
-->
---
layout: lesson
ratio: balanced
---

::title::

# CNNs build global context *gradually*

::text::

- **Convolution is local by design**  
  Each layer combines neighbouring pixels or features

- **Depth grows the receptive field**  
  Distant regions can influence one another, but only after several layers

- **Local ambiguity can require distant context**  
  The question is not whether CNNs can use global context, but how they acquire it

> CNNs can leverage distant context, but only after building it up through depth

::visual::

<CnnContextGrowth />

<!--
**Purpose:** Reconnect receptive fields to the need for context without presenting CNNs as a failed architecture

**Say:** Locality and weight sharing are useful biases. The contrast is direct access versus gradual composition, not local versus global capability.

**Transition:** What would it look like if a region could use a distant cue directly?
-->
---
layout: lesson
ratio: visual
---

::title::

# *Attention* lets distant regions interact directly

::text::

- **Missing context**  
  A crop around the person of interest does not reveal the situation.
  
- **Integrate clues**  
  The wider scene reveals a *dangerous situation for the pedestrian.*

- **Useful context**  
  These regions can have different shapes and sizes, and may be far apart in the image.

> How can a model find the relevant context regions before it understands the image?

::visual::

<DirectContextScene />

<div class="source-note">Marek Ślusarczyk · Wikimedia Commons · CC BY 3.0</div>

<!--
**Purpose:** Establish the desired information flow without naming attention mechanics

**Say:** We are recognising an observable red-light crossing, not inferring an internal intention. The meaningful regions are attractive but impractical: choosing them already requires scene understanding.

**Transition:** How can an encoder start from a fixed, manageable set of image regions instead?
-->
---
layout: lesson
ratio: visual
---

::title::

# What are the *units* of visual attention?

::text::

- **Semantic regions** are unavailable before understanding the scene

- **One whole image** loses local detail

- **Pixels** create too many units

- A regular grid of **fixed-size patches** covers the whole image without preselecting objects


> Patches give attention a fixed, practical set of regions to compare.

::visual::

<PatchGridScene mode="grid" />

<!--
**Purpose:** Introduce fixed-size patches as a practical initial unit, not as object proposals

**Say:** These 32 × 32 patches are deliberately coarse for lecture readability. A patch need not align to an object. At the end, name the learned vector for a patch a token.

**Transition:** Patches are now tokens. How does a Vision Transformer process them?
-->
---
layout: lesson
ratio: visual
---

::title::

# What is a *Vision Transformer*?

::text::


- **Transformer, adapted for images**<br>
  + Original Transformer: **word tokens**
  + Vision Transformer: **image-patch tokens**

- **Repeated encoder blocks**<br>
  + **Self-attention** mixes information across patches.
  + An **MLP** transforms each enriched patch representation.


> Once an image is converted into tokens, they can exchange information.

::visual::

<figure class="paper-figure">
  <img src="/images/vit/dosovitskiy-figure-1.png" alt="Original Vision Transformer architecture diagram showing image patches, linear patch embeddings, positional embeddings, Transformer encoder, classification token, and MLP head" />
  <figcaption>Dosovitskiy et al. (2021), Figure 1 · “An Image is Worth 16×16 Words” · CC BY 4.0</figcaption>
</figure>

<!--
**Purpose:** Give the complete architectural map before unpacking self-attention

**Say:** This adapts the Transformer; it does not claim that images are language. The input units change from words to patches. The self-attention box is what we unpack next.

**Class token:** The learnable class embedding is a single learned input vector shared by every image; it is not extracted from the image and does not pass through the patch-projection step. The `*` in Figure 1 marks its own learned positional embedding, not multiplication. Once inside the encoder, the class token attends to all image tokens. Its final representation therefore becomes image-specific and is passed to the classification MLP.

**Depth:** The `× L` label means that the encoder block is repeated `L` times; this is the model depth. Standard original ViT configurations use 12 layers for ViT-Base, 24 for ViT-Large, and 32 for ViT-Huge. Each layer contains multi-head self-attention followed by an MLP.

**Transition:** Before attention can compare tokens, how does it know where each patch came from?
-->
---
layout: lesson
ratio: visual
---

::title::

# From patches to *input tokens*

::text::

- **Patch embedding**  
  A learned mapping turns each patch into a vector, it describes the local visual content
- **Position embedding**  
  Each patch location has its own learned vector.
  The model learns a useful representation of location from data.
- **Input token**  
  Add both embedding vectors to form one representation

> A token combines *what* a patch contains with *where* it came from.

::visual::

<PositionalEncodingFlow />

<!--
**Purpose:** Explain why spatial information must be explicitly added before attention processes patch tokens

**Say:** Positional embeddings do not recreate pixels. They give the model location information from which it can learn spatial relationships.

**Transition:** With content and position available, each region can ask which other regions matter.
-->
---
layout: lesson
ratio: visual
---

::title::

# Self-attention lets patches *exchange information*

::text::

- Every input token produces three learned vectors:
  - **Query:** what this patch is looking for
  - **Key:** what this patch offers for matching
  - **Value:** the information this patch can contribute


> Each patch gathers information from the patches most relevant to it, not necessarily its spatial neighbors.

::visual::

<SelfAttentionMechanismRevised />

<!--
**Purpose:** Zoom from the architecture’s self-attention block into one focal-patch example

**Say:** Q, K, and V are learned projections of the same token, not hand-authored semantic labels. The full matrix computation updates every patch at once.

**Transition:** Return to the complete architecture and locate these familiar mechanisms.
-->
---
layout: lesson
ratio: visual
---

::title::

# From image patches to *prediction*

::text::

- **Encode the image**
  patches + position-aware tokens + repeated Transformer layers

- **Build context**
  Transformer layers make each patch representation context-aware

- **Adapt to a task**
  Small MLP head on top of final representation

> **Same transfer idea as CNNs:** reuse the pretrained encoder




::visual::

<figure class="paper-figure">
  <img src="/images/vit/dosovitskiy-figure-1.png" alt="Original Vision Transformer architecture diagram showing image patches, linear patch embeddings, positional embeddings, Transformer encoder, classification token, and MLP head" />
  <figcaption>Dosovitskiy et al. (2021), Figure 1 · “An Image is Worth 16×16 Words” · CC BY 4.0</figcaption>
</figure>

<!--
**Purpose:** Connect the ViT encoder to the familiar transfer-learning pattern: pretrained encoder plus a task-specific head

**Say:** The diagram is now a map of an image encoder: patches become position-aware tokens, and repeated Transformer layers contextualize them. For classification, the final class-token representation goes to a small MLP head. This is the same transfer pattern as CNNs: reuse the pretrained encoder and replace or fine-tune the task head.

**Historical aside:** In NLP, processing all tokens together with dense matrix operations helped Transformers scale beyond sequential recurrent models. Full attention still compares every token pair, so its cost grows quadratically with sequence length.

**Transition:** What do these learned components look like after the model has trained?
-->
---
layout: lesson
ratio: visual
---

::title::

# What does a trained *Vision Transformer* learn?

::text::

- **Patch projection:**  
  Learns color and orientation sensitive visual features
- **Position embeddings:**  
  Learn spatial relationships between patch locations
- **Attention heads:**  
  Specialize in different interaction ranges across the image

> **A trained ViT learns *what* to look for, *where* it is, and how *far* to look for context.**

::visual::

<figure class="paper-figure">
  <img src="/images/vit/dosovitskiy-figure-7.png" alt="Dosovitskiy et al. Figure 7 showing learned RGB patch-projection filters, position-embedding cosine similarity maps, and mean attention distance for heads across Transformer layers" />
  <figcaption>Dosovitskiy et al. (2021), Figure 7 · “An Image is Worth 16×16 Words” · CC BY 4.0</figcaption>
</figure>

<!--
**Purpose:** Examine evidence of what ViT components learn after training

**Say:** Introduce Dosovitskiy et al. Figure 7 from left to right. First, visualizations of the initial patch-projection filters; then position-embedding similarity; finally, attention range across heads and depth. Detailed explanation will be refined in the next pass.

**Transition:** These learned mechanisms reveal a contrast in how CNNs and ViTs are biased to represent images.
-->
---
layout: comparison
---

::title::

# Architecture shapes *representation*

::left::

### CNN encoder

- Locality and shared filters are built-in priors
- Broad context grows gradually through depth
- Spatial structure is supplied by the architecture

::right::

### ViT encoder

- Patches, explicit position, and direct token interaction
- Image-wide, content-dependent interaction in every attention layer
- More spatial structure must be learned from data

::takeaway::

> In both cases: pretraining can produce a reusable encoder; a new task adds or fine-tunes a small prediction head.


<!--
**Purpose:** Close the section by contrasting the two encoder biases and stating what a ViT changes in visual representation learning

**Say:** CNNs build image context gradually through local layers; ViTs make image-wide exchange direct. Both produce reusable encoders followed by task heads. Neither bias is automatically best: the choice depends on task, resolution, data, objective, scale, and compute. The global message is that a ViT represents an image as tokens whose representations repeatedly exchange context.

**Transition:** Move on from encoder architecture; later sections address learning objectives and pretraining.
-->
