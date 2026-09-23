---
layout: section
number: 07
---

::title::

# Large-Scale *Visual Pretraining*

::question::

What changes when visual representations are learned at scale?

<!--
**Purpose:** Move from learning objectives to what expanded pretraining resources can make those objectives support

**Say:** Section 6 compared what a model is asked to predict. This section asks what becomes possible when data diversity, model capacity, and training budget grow

**Transition:** First frame scale as a changed training regime, not a promise of universal performance
-->
---
layout: lesson
ratio: balanced
---

::title::

# Scale changes the *pretraining regime*

::text::

- **More diverse data**  
  The encoder can encounter a wider range of visual variation

- **More model capacity**  
  A larger encoder can store and combine more learned structure

- **More training budget**  
  Optimization can expose the model to that data for longer

> Scale can broaden transferability, but it does not remove task dependence completely

::visual::

<ScalePretrainingRegime />

<!--
**Purpose:** Provide a cautious definition of scale before two paper-led examples

**Say:** Do not teach scaling laws here. Treat scale as the combined data, model, and optimization regime that changes the range of uses worth testing

**Transition:** First examine a self-supervised visual encoder trained in such a regime
-->
---
layout: lesson
ratio: balanced
---

::title::

# DINOv2 learns *reusable visual features* at scale

::text::

- **Increase the scale with respect to DINO**  
  Dataset : 142M curated images vs ~1.3M (ImageNet)  
  Model : ViT-G ~1.1B parameters vs ViT-S/B ~10M

- **Improved, more complex objective**  
  Add iBOT procedure: mask some tokens for student  
  Teacher sees all tokens, student must predict the latent representation

- **New learning paradigm**  
  From mostly global semantics to local (patch-level) semantics

> DINO proved that SSL works,  
> DINOv2 builds a vision foundation model

::visual::

<figure class="paper-figure">
  <img src="/images/large-scale/dinov2.png" alt="DINOv2 figure illustrating the learned visual features and their reuse across tasks" />
</figure>

<div class="source-note">Oquab et al., 2023 · “DINOv2: Learning Robust Visual Features without Supervision”</div>

<!--
**Purpose:** Make a large-scale self-supervised visual backbone concrete without claiming universal visual understanding

**Say:** When figures are added, use one legible qualitative result and read it precisely. It is evidence about a particular capability, not a benchmark-table tour or a universal ranking

**Transition:** Scale can also change the representation's interface by aligning visual features with language
-->
---
layout: lesson
ratio: balanced
---

::title::

# CLIP connects *images and language*

::text::

- **Paired images and text provide the training signal**  
  Use image-text pairs to learn image and text encoders.   
  Embeddings are learned using contrastive examples.

- **Both encoders share an embedding space**  
  Matching pairs are trained to be close together.  
  A similarity matrix helps measure their agreement.

- **Text becomes a query interface**  
  Compare an image embedding with text prompts for retrieval or zero-shot classification

> CLIP connects vision and language through contrastive learning.

::visual::

<figure class="paper-figure">
  <img src="/images/large-scale/clip.png" alt="CLIP figure showing aligned image and text embeddings and the shared representation space" />
</figure>

<div class="source-note">Radford et al., 2021 · “Learning Transferable Visual Models From Natural Language Supervision”</div>

<!--
**Purpose:** Explain image-text alignment and its resulting query interface

**Say:** A text prompt is not a fixed task classifier. Results can change with wording, vocabulary coverage, and domain. Use this distinction to contrast CLIP with DINOv2's visual-only backbone

**Transition:** Close by choosing between a visual backbone and an image-text embedding from the actual downstream need
-->
---
layout: lesson
---

::title::

# Choose the *right representation*

::text::

- **Visual backbone**  
  Use it when the downstream task depends on dense visual structure, local detail, or similarity in image space

- **Image-text embedding**  
  Use it when the query is expressed in language and the task is retrieval or open-vocabulary matching

- **Scale matters, but it does not replace the task**  
  Pretraining expands what is reusable, yet the final test remains the real downstream objective

> The useful representation is the one whose learning signal, scale, and interface match the task

---
layout: comparison
---

::title::

# Final takeaways

::left::

- **Image mining starts from the right representation**  
  The key question is not only how to extract features, but which representation matches the mining task

- **Handcrafted features are interpretable but brittle**  
  They encode a fixed notion of similarity and do not adapt to the data distribution

- **Learned features are data-driven and reusable**  
  Deep models turn pixels into structured embeddings that can transfer across tasks

::right::

- **Self-supervised and multimodal objectives broaden the signal**  
  Contrastive, predictive, masked, and language-aligned objectives reveal more useful structure from unlabeled or weakly labeled images

- **The task still defines the right model**  
  In image mining, the useful representation depends on the target question: similarity, retrieval, classification, or semantic search

::takeaway::

> In image mining, the goal is not to find the single best feature extractor, but the representation that makes the right visual evidence easy to retrieve, compare, and reuse
