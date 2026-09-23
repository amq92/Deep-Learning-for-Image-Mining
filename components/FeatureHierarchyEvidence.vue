<template>
  <figure class="hierarchy-evidence" aria-label="Layer-wise feature visualizations from Zeiler and Fergus">
    <div class="evidence-stage">
      <div v-click-hide="1" class="layer-group early-layers">
        <div class="layer-panel compact">
          <span>Layer 1</span>
          <img src="/images/representations/zeiler-fergus/layer-1.jpg" alt="Layer 1 feature visualizations showing oriented edges and colors" />
        </div>
        <div class="layer-panel">
          <span>Layer 2</span>
          <img src="/images/representations/zeiler-fergus/layer-2.jpg" alt="Layer 2 feature visualizations showing textures and repeated patterns" />
        </div>
      </div>

      <div v-click="1" class="layer-group deep-layers">
        <div class="layer-panel">
          <span>Layer 3</span>
          <img src="/images/representations/zeiler-fergus/layer-3.jpg" alt="Layer 3 feature visualizations showing richer textures and shapes" />
        </div>
        <div class="layer-panel">
          <span>Layer 4</span>
          <img src="/images/representations/zeiler-fergus/layer-4.jpg" alt="Layer 4 feature visualizations showing object parts and structures" />
        </div>
        <div class="layer-panel">
          <span>Layer 5</span>
          <img src="/images/representations/zeiler-fergus/layer-5.jpg" alt="Layer 5 feature visualizations showing complex parts and objects" />
        </div>
      </div>
    </div>

    <figcaption>
      Adapted from Zeiler & Fergus, <em>Visualizing and Understanding Convolutional Networks</em>, ECCV 2014, Fig. 2
    </figcaption>
  </figure>
</template>

<style scoped>
.hierarchy-evidence { display: flex; flex-direction: column; height: 100%; justify-content: center; margin: 0; min-width: 0; width: 100%; }
.evidence-stage { display: grid; min-height: 0; width: 100%; }
.evidence-stage > * { grid-area: 1 / 1; }
.layer-group { align-content: center; display: grid; gap: 0.55rem; min-width: 0; }
.early-layers { grid-template-rows: minmax(0, 0.8fr) minmax(0, 1fr); }
.deep-layers { gap: 0.42rem; }
.layer-panel { min-width: 0; position: relative; }
.layer-panel span { background: var(--accent); border-radius: 999px; color: white; font-size: 0.58rem; font-weight: 800; left: 0.45rem; letter-spacing: 0.04em; padding: 0.18rem 0.48rem; position: absolute; top: 0.4rem; z-index: 1; }
.layer-panel img { background: #000; border: 1px solid var(--line); border-radius: 0.65rem; box-shadow: 0 6px 18px var(--shadow); display: block; height: 100%; max-height: 10.2rem; object-fit: contain; width: 100%; }
.layer-panel.compact img { max-height: 8rem; }
.deep-layers .layer-panel img { max-height: 7.35rem; }
.hierarchy-evidence figcaption { color: var(--muted); font-size: 0.5rem; line-height: 1.25; margin-top: 0.48rem; text-align: center; }
</style>
