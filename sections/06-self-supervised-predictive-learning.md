---
layout: section
number: 06
---

::title::

# Self-Supervised and *Predictive Learning*

::question::

How can useful visual representations be learned without annotations?

<!--
**Purpose:** Shift from the encoder architecture to the learning signal that shapes its representations

**Say:** CNNs and ViTs determine how information can interact. The next question is what objective teaches an encoder which image structure to preserve

**Transition:** First distinguish a human-provided target from a target constructed from the image itself
-->
---
layout: lesson
ratio: balanced
---

::title::

# Self-supervision creates a *target from data*

::text::

- **Supervised learning**  
  The model learns to predict a known class or value from annoted data.  

- **Self-supervised learning**  
  The model constructs its target from the unlabeled data.
  Another view, hidden content, or a teacher representation can provide it

- **The objective still matters**  
  It rewards the encoder for preserving some image structure rather than others

> No human label does not mean no learning signal

::visual::

<SelfSupervisionTargetComparison />

<!--
**Purpose:** Give self-supervision one clear definition before individual paper case studies

**Say:** The image supplies the information used to make a target, but the learning objective decides what agreement or prediction the encoder is rewarded for

**Transition:** Start with a method that makes two transformed views of one image agree
-->
---
layout: lesson
ratio: balanced
---

::title::

# DINO matches *different views* of one image

::text::
- **Self-*di*stillation with *no* labels (DINO)**  
  Semantic identity is preserved under transformations

- **Two observations of the same source image**  
  Student and teacher receive different augmented views

- **The student matches the teacher output**  
  Agreement makes the representation less sensitive to the chosen view

- **The teacher is not a label source**  
  A slowly updated copy of the student that supplies stable targets

- **Avoiding representation collapse**  
  DINO’s training dynamics discourage trivial representations

::visual::

<div class="dino-paper-sequence">
  <figure class="paper-figure" v-click-hide="1">
    <img src="/images/self-supervised/dino/figure-2.png" alt="DINO paper Figure 2 showing the self-distillation training setup with student and teacher networks" />
  </figure>

  <figure class="paper-figure" v-click-hide="2" v-click="1">
    <div class="dino-figure-frame">
      <img src="/images/self-supervised/dino/figure-3.png" alt="DINO paper Figure 3 showing the multi-crop training strategy and teacher-student view assignments" />
    </div>
  </figure>

  <figure class="paper-figure dino-zoomed-figure" v-click="2">
    <div class="dino-zoom-frame">
      <img src="/images/self-supervised/dino/figure-3.png" alt="Zoomed view of DINO paper Figure 3 showing the multi-crop training strategy and teacher-student view assignments" />
    </div>
  </figure>
</div>

<div class="source-note">Caron et al., 2021 · “Emerging Properties in Self-Supervised Vision Transformers”</div>

<style>
.dino-paper-sequence {
  display: grid;
  min-height: 18rem;
  place-items: center;
  width: 100%;
}

.dino-paper-sequence > .paper-figure {
  grid-area: 1 / 1;
  width: min(100%, 39.1rem);
}

.dino-paper-sequence .paper-figure img {
  max-height: 21.85rem;
  padding: 0.35rem 0.5rem;
  width: 96%;
  margin-inline: auto;
}

.dino-figure-frame,
.dino-zoom-frame {
  background: white;
  border: 1px solid var(--line);
  border-radius: 12px;
  box-shadow: 0 8px 22px var(--shadow);
  box-sizing: border-box;
  height: 21.85rem;
  margin-inline: auto;
  overflow: hidden;
  padding: 0.35rem 0.5rem;
  width: 96%;
}

.dino-figure-frame img,
.dino-zoom-frame img {
  background: transparent;
  border: 0;
  border-radius: 0;
  box-shadow: none;
  box-sizing: border-box;
  display: block;
  height: 100%;
  object-fit: contain;
  padding: 0;
  width: 100%;
}

.dino-paper-sequence > .dino-zoomed-figure {
  opacity: 1;
  transform: scale(1);
  transform-origin: center center;
  transition: opacity 450ms ease, transform 450ms ease;
}

.dino-paper-sequence > .dino-zoomed-figure.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.86);
}

.dino-zoom-frame img {
  object-fit: contain;
  transform: scale(1.7);
  transform-origin: center top;
}
</style>

<!--
**Purpose:** Explain DINO's label-free agreement target without treating the teacher as an oracle

**Say:** The exponential-moving-average update makes the teacher change more slowly than the student. The augmentation choice defines which changes should not alter the representation

**Transition:** A different target can instead ask the encoder to recover content it cannot see
-->
---
layout: lesson
ratio: balanced
---

::title::

# MAE reconstructs *what was hidden*

::text::

- **Mask many image patches**  
  The encoder receives only the visible patches (~25%)

- **Reconstruct the missing pixels**  
  The missing patches are provided as *masked* tokens  
  A lightweight decoder learns to recover the hidden image content  

- **Keep the encoder after pretraining**  
  The decoder mainly serves the reconstruction objective

> MAE learns by asking the encoder to explain image content it cannot see

::visual::

<div class="mae-paper-sequence">
  <figure class="paper-figure" v-click-hide="1">
    <img src="/images/self-supervised/mae/figure-1.png" alt="MAE paper figure showing masked image patches presented to the encoder and reconstructed by the decoder" />
  </figure>

  <figure class="paper-figure" v-click-hide="2" v-click="1">
    <div class="mae-figure-frame">
      <img src="/images/self-supervised/mae/figure-2.png" alt="MAE paper figure showing the asymmetric encoder-decoder architecture and masked patch reconstruction" />
    </div>
  </figure>

  <figure class="paper-figure mae-zoomed-figure" v-click="2">
    <div class="mae-zoom-frame">
      <img src="/images/self-supervised/mae/figure-2.png" alt="Zoomed view of the MAE paper figure showing the asymmetric encoder-decoder architecture and masked patch reconstruction" />
    </div>
  </figure>
</div>

<style>
.mae-paper-sequence {
  display: grid;
  min-height: 18rem;
  place-items: center;
  width: 100%;
}

.mae-paper-sequence > .paper-figure {
  grid-area: 1 / 1;
  width: min(100%, 39.1rem);
}

.mae-paper-sequence .paper-figure > img {
  max-height: 21.85rem;
  padding: 0.35rem 0.5rem;
  width: 96%;
  margin-inline: auto;
}

.mae-figure-frame,
.mae-zoom-frame {
  background: white;
  border: 1px solid var(--line);
  border-radius: 12px;
  box-shadow: 0 8px 22px var(--shadow);
  box-sizing: border-box;
  height: 21.85rem;
  margin-inline: auto;
  overflow: hidden;
  padding: 0.35rem 0.5rem;
  width: 96%;
}

.mae-paper-sequence > .mae-zoomed-figure {
  opacity: 1;
  transform: scale(1);
  transform-origin: center center;
  transition: opacity 450ms ease, transform 450ms ease;
}

.mae-paper-sequence > .mae-zoomed-figure.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.86);
}

.mae-figure-frame img {
  background: transparent;
  border: 0;
  border-radius: 0;
  box-shadow: none;
  box-sizing: border-box;
  display: block;
  height: 100%;
  object-fit: contain;
  padding: 0;
  width: 100%;
}

.mae-zoom-frame img {
  background: transparent;
  border: 0;
  border-radius: 0;
  box-shadow: none;
  box-sizing: border-box;
  display: block;
  max-height: none;
  padding: 0;
  transform: scale(2.15);
  transform-origin: center top;
  width: 100%;
}
</style>

<div class="source-note">He et al., 2022 · “Masked Autoencoders Are Scalable Vision Learners”</div>

<!--
**Purpose:** Establish pixel reconstruction as an intuitive masked-learning objective

**Say:** The decoder is useful during pretraining, but the encoder is the component usually reused for downstream tasks. Reconstruction does not itself prove that every feature is semantic

**Transition:** The next method also hides image regions, but changes the prediction target from pixels to a representation
-->
---
layout: lesson
ratio: balanced
---

::title::

# I-JEPA predicts the *missing meaning*

::text::

- **Context remains visible**  
  The model sees only part of the image and must infer the rest

- **Predict a latent target**  
  Instead of reconstructing pixels, it predicts the representation of a hidden region

- **Why this matters**  
  The goal is to learn the abstract structure that makes a scene meaningful, not to copy pixels  
  Hence, the model learns semantic features.

> I-JEPA asks: what latent information should be preserved from partial observations?

::visual::

<div class="jepa-paper-sequence">
  <figure class="paper-figure" v-click-hide="1">
    <div class="jepa-figure-frame">
      <img src="/images/self-supervised/jepa/figure-1.png" alt="I-JEPA paper figure showing visible context patches and target prediction task" />
    </div>
  </figure>

  <figure class="paper-figure" v-click-hide="2" v-click="1">
    <div class="jepa-figure-frame">
      <img src="/images/self-supervised/jepa/figure-2.png" alt="I-JEPA paper figure showing context encoder and predictor for hidden target representations" />
    </div>
  </figure>

  <figure class="paper-figure" v-click-hide="3" v-click="2">
    <div class="jepa-figure-frame">
      <img src="/images/self-supervised/jepa/figure-3.png" alt="I-JEPA paper figure showing the final latent-prediction target architecture" />
    </div>
  </figure>

  <figure class="paper-figure jepa-zoomed-figure" v-click="3">
    <div class="jepa-zoom-frame">
      <img src="/images/self-supervised/jepa/figure-3.png" alt="Zoomed detail of I-JEPA paper figure 3 showing the latent prediction architecture" />
    </div>
  </figure>
</div>

<style>
.jepa-paper-sequence {
  display: grid;
  min-height: 18rem;
  place-items: center;
  width: 100%;
}

.jepa-paper-sequence > .paper-figure {
  grid-area: 1 / 1;
  width: min(100%, 39.1rem);
}

.jepa-paper-sequence .paper-figure > img {
  max-height: 21.85rem;
  padding: 0.35rem 0.5rem;
  width: 96%;
  margin-inline: auto;
}

.jepa-figure-frame,
.jepa-zoom-frame {
  background: white;
  border: 1px solid var(--line);
  border-radius: 12px;
  box-shadow: 0 8px 22px var(--shadow);
  box-sizing: border-box;
  height: 21.85rem;
  margin-inline: auto;
  overflow: hidden;
  padding: 0.35rem 0.5rem;
  width: 96%;
}

.jepa-paper-sequence > .jepa-zoomed-figure {
  opacity: 1;
  transform: scale(1);
  transform-origin: center center;
  transition: opacity 450ms ease, transform 450ms ease;
}

.jepa-paper-sequence > .jepa-zoomed-figure.slidev-vclick-hidden {
  opacity: 0;
  transform: scale(0.86);
}

.jepa-figure-frame img,
.jepa-zoom-frame img {
  background: transparent;
  border: 0;
  border-radius: 0;
  box-shadow: none;
  box-sizing: border-box;
  display: block;
  height: 100%;
  object-fit: contain;
  padding: 0;
  width: 100%;
}

.jepa-zoom-frame img {
  transform: translateX(-0.8rem) translateY(5rem) scale(2.1);
  transform-origin: left center;
}
</style>

<div class="source-note">Assran et al., 2023 · “Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture”</div>

<!--
**Purpose:** Make the difference between pixel reconstruction and latent prediction explicit

**Say:** The target encoder is updated as an exponential-moving-average teacher. The objective does not demand a pixel decoder, but it does not guarantee that all image detail is discarded

**Transition:** Compare the three methods only through the target each one asks an encoder to predict
-->
---
layout: comparison
---

::title::

# The objective sets *what to preserve*

::left::

### DINO

- **Target:** another view's representation
- **Pressure:** agree across chosen augmentations
- **Useful lens:** invariance to image changes

### MAE

- **Target:** missing pixel patches
- **Pressure:** preserve content for reconstruction
- **Useful lens:** recover hidden image content

::right::

### I-JEPA

- **Target:** a hidden region's representation
- **Pressure:** predict high-level structure from context
- **Useful lens:** latent prediction without pixel reconstruction

::takeaway::

> The learning objective decides which image structure an encoder is rewarded for preserving

<!--
**Purpose:** Synthesize the three paper cases as different target choices, not a performance ranking

**Ask:** Which method explicitly reconstructs pixels? MAE

**Transition:** Labels are no longer the only limiting resource. Next ask what changes when these learning strategies are trained at a much larger scale
-->
---
layout: lesson
---

::title::

# Self-supervision learns *transferable features*

::text::

- **Different targets, same purpose**  
  DINO, MAE, and I-JEPA all train an encoder without labels

- **The point is the representation**  
  The encoder learns features that should be useful beyond the pretraining task  
  A pretrained backbone can then be adapted to multiple tasks

- **This is why the objective matters**  
  Each method preserves a different kind of visual structure: invariance, content, or latent meaning

> These methods build pretrained features that can be reused in downstream tasks


<!--
**Purpose:** End the section by connecting the objective comparison to the real downstream payoff: reusable pretrained features

**Say:** The value of these methods is not just that they train without labels. It is that they produce encoders whose features can be reused on new tasks, exactly as in the transfer-learning examples we saw earlier
-->
