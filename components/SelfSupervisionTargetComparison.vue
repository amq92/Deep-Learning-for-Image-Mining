<template>
  <figure class="self-supervision-comparison" aria-label="Three self-supervised learning objectives using the same duck image">
    <section class="method-panel">
      <div class="method-header">
        <span class="method-name">1 · DINO</span>
        <small>agreement</small>
      </div>

      <div class="mini-scene ijepa-scene dino-scene" aria-label="Two views of the same image connected by matching representation matrices">
        <div class="ijepa-image left-image">
          <img src="/images/representation/duck-primary.jpg" alt="Primary duck image" />
        </div>
        <div class="ijepa-connector left-connector">→</div>
        <div class="link-matrix left-matrix">
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
        </div>
        <div class="ijepa-arrow dino-center-arrow">↔</div>
        <div class="link-matrix right-matrix">
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
        </div>
        <div class="ijepa-connector right-connector">←</div>
        <div class="ijepa-image right-image dino-right-image">
          <img src="/images/representation/duck-primary.jpg" alt="Primary duck image" />
        </div>
      </div>

      <p>same image, two views → agree on the representation</p>
    </section>

    <section class="method-panel">
      <div class="method-header">
        <span class="method-name">2 · MAE</span>
        <small>reconstruction</small>
      </div>

      <div class="mini-scene mae-scene" aria-label="Masked duck image reconstructed into a clean duck image">
        <div class="mae-image-wrap">
          <img src="/images/representation/duck-primary.jpg" alt="Duck image with random masked patches" />
          <div class="mask-overlay" aria-hidden="true">
            <span class="mask-3"></span>
            <span class="mask-4"></span>
            <span class="mask-5"></span>
            <span class="mask-6"></span>
          </div>
        </div>
        <div class="mae-arrow" aria-hidden="true">→</div>
        <div class="mae-image-wrap clean-image">
          <img src="/images/representation/duck-primary.jpg" alt="Clean reconstructed duck image" />
        </div>
      </div>

      <p>mask a few regions → recover the missing pixels</p>
    </section>

    <section class="method-panel">
      <div class="method-header">
        <span class="method-name">3 · I-JEPA</span>
        <small>prediction</small>
      </div>

      <div class="mini-scene ijepa-scene" aria-label="Two duck images connected by matching block matrices that represent the context and the hidden target">
        <div class="ijepa-image left-image">
          <img src="/images/representation/duck-primary.jpg" alt="Context duck image" />
        </div>
        <div class="ijepa-connector left-connector">→</div>
        <div class="link-matrix left-matrix">
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
        </div>
        <div class="ijepa-arrow jepa-center-arrow">→</div>
        <div class="link-matrix right-matrix">
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
          <i></i><i></i><i></i>
        </div>
        <div class="ijepa-connector right-connector">←</div>
        <div class="ijepa-image right-image">
          <img src="/images/representation/duck-alternate.jpg" alt="Target duck image" />
        </div>
      </div>

      <p>context image ↔ hidden target block</p>
    </section>
  </figure>
</template>

<style scoped>
.self-supervision-comparison {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.7rem;
  width: 75%;
  justify-self: center;
  margin: 0;
}

.method-panel {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  min-width: 0;
  padding: 0.48rem 0.6rem 0.45rem;
}

.method-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 0.38rem;
}

.method-name {
  color: var(--accent);
  font-size: 0.62rem;
  font-weight: 800;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.method-header small {
  color: var(--muted);
  font-size: 0.46rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}

.mini-scene {
  position: relative;
  border: 0;
  border-radius: 0;
  background: transparent;
  height: 4.6rem;
  overflow: visible;
}

.method-panel p {
  margin: 0.34rem 0 0;
  color: var(--muted);
  font-size: 0.53rem;
  line-height: 1.25;
}

.image-tile {
  overflow: hidden;
  border: 0;
  border-radius: 0;
  background: transparent;
  height: 100%;
  min-height: 0;
}

.image-tile img,
.ijepa-image img,
.mae-scene img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: center;
  background: transparent;
  border-radius: 0;
}

.arrow-hub,
.ijepa-arrow {
  color: var(--accent);
  font-size: 1.1rem;
  font-weight: 800;
  line-height: 1;
}

.mae-scene {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  padding: 0;
}

.mae-image-wrap {
  position: relative;
  width: 2.59rem;
  height: 4.6rem;
  overflow: hidden;
}

.mae-arrow {
  color: var(--accent);
  font-size: 1.1rem;
  font-weight: 800;
  line-height: 1;
}

.mask-overlay {
  position: absolute;
  inset: 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(5, 1fr);
  gap: 0.14rem;
  padding: 0.18rem;
}

.mask-overlay span {
  display: block;
  background: #3e78b8;
  border: 1px solid #fff;
  border-radius: 2px;
}

.mask-1 { grid-column: 1; grid-row: 1; }
.mask-2 { grid-column: 3; grid-row: 1; }
.mask-3 { grid-column: 2; grid-row: 2; }
.mask-4 { grid-column: 3; grid-row: 3; }
.mask-5 { grid-column: 1; grid-row: 4; }
.mask-6 { grid-column: 2; grid-row: 5; }

.ijepa-scene {
  display: grid;
  grid-template-columns: 1fr 0.38fr 1fr 0.28fr 1fr 0.38fr 1fr;
  align-items: center;
  justify-items: center;
  gap: 0.12rem;
  padding: 0.25rem;
  width: 100%;
}

.ijepa-image,
.ijepa-target {
  overflow: hidden;
  border: 0;
  border-radius: 0;
  background: transparent;
  height: 100%;
  min-height: 0;
  position: relative;
  width: 2.5rem;
  justify-self: center;
}

.ijepa-image img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 0;
}

.dino-right-image {
  transform: rotate(4deg) skewX(-8deg);
}

.ijepa-connector {
  color: var(--accent);
  font-size: 0.85rem;
  font-weight: 800;
  line-height: 1;
  justify-self: center;
  pointer-events: none;
}

.left-connector {
  grid-column: 2;
}

.right-connector {
  grid-column: 6;
}

.link-matrix {
  display: grid;
  grid-template-columns: repeat(3, 0.42rem);
  gap: 0.08rem;
  padding: 0.14rem;
  border-radius: 4px;
  background: #f9e8ea;
  box-shadow: inset 0 0 0 1px rgba(153, 48, 57, 0.22);
  justify-self: center;
}

.link-matrix i {
  display: block;
  width: 0.42rem;
  height: 0.42rem;
  border-radius: 2px;
}

.left-matrix {
  grid-column: 3;
  background: #edf4ff;
  box-shadow: inset 0 0 0 1px rgba(74, 110, 180, 0.24);
}

.left-matrix i {
  background: #6c8fd8;
}

.right-matrix {
  grid-column: 5;
  background: #fdf0e9;
  box-shadow: inset 0 0 0 1px rgba(170, 103, 54, 0.24);
}

.right-matrix i {
  background: #d98c4a;
}

.ijepa-arrow {
  grid-column: 4;
  color: var(--accent);
  font-size: 0.9rem;
  font-weight: 800;
  line-height: 1;
  justify-self: center;
}

.jepa-center-arrow {
  color: #2f7f91;
}
</style>
