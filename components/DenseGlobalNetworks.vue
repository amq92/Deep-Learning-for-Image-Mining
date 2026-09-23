<template>
  <figure class="comparison" aria-label="Comparison of global classification with ResNet and dense segmentation with U-Net">
    <section class="network-card global-card">
      <header>
        <strong>ResNet</strong>
        <span>classification</span>
      </header>
      <div class="pipeline">
        <div class="image input-image" aria-label="input image">
          <span class="object"></span>
        </div>
        <span class="arrow">→</span>
        <div class="encoder">
          <span v-for="n in 4" :key="n"></span>
        </div>
        <span class="arrow">→</span>
        <div class="vector" aria-label="global feature vector">
          <i v-for="n in 8" :key="n"></i>
        </div>
      </div>
      <p>Global pooling removes the spatial grid</p>
      <div class="output-label">one image-level prediction</div>
    </section>

    <section class="network-card dense-card">
      <header>
        <strong>U-Net</strong>
        <span>segmentation</span>
      </header>
      <div class="unet-wrap">
        <div class="image input-image" aria-label="input image">
          <span class="object"></span>
        </div>
        <span class="arrow">→</span>
        <div class="unet" aria-label="encoder decoder with skip connections">
          <div class="arm left"><i></i><i></i><i></i></div>
          <div class="bottleneck"></div>
          <div class="arm right"><i></i><i></i><i></i></div>
          <span class="skip skip-one"></span>
          <span class="skip skip-two"></span>
        </div>
        <span class="arrow">→</span>
        <div class="image mask" aria-label="pixel-level segmentation mask">
          <span class="object"></span>
        </div>
      </div>
      <p>Spatial maps and skip connections preserve location</p>
      <div class="output-label">one prediction per pixel</div>
    </section>

    <figcaption>Architectures simplified from He et al. (2016) and Ronneberger et al. (2015)</figcaption>
  </figure>
</template>

<style scoped>
.comparison { display: grid; gap: 0.7rem; margin: 0; width: 100%; }
.network-card { background: var(--surface); border: 1px solid var(--line); border-radius: 0.8rem; box-shadow: 0 7px 20px var(--shadow); padding: 0.72rem 0.8rem; }
.network-card header { align-items: baseline; display: flex; gap: 0.5rem; margin-bottom: 0.55rem; }
.network-card header strong { color: var(--accent); font-size: 0.93rem; }
.network-card header span { color: var(--muted); font-size: 0.58rem; font-weight: 700; text-transform: uppercase; }
.pipeline, .unet-wrap { align-items: center; display: flex; justify-content: center; min-height: 4.8rem; }
.arrow { color: var(--muted); font-size: 1rem; margin: 0 0.3rem; }
.image { background: #111; border: 1px solid #555; border-radius: 0.35rem; height: 3.25rem; overflow: hidden; position: relative; width: 3.25rem; }
.input-image { background: linear-gradient(145deg, #172131, #0b0d11); }
.object { background: #ef8354; border-radius: 48% 48% 40% 40%; height: 1.75rem; left: 0.75rem; position: absolute; top: 0.75rem; transform: rotate(-12deg); width: 1.7rem; }
.encoder { align-items: center; display: flex; gap: 0.14rem; }
.encoder span { background: color-mix(in srgb, var(--accent) 65%, #182333); border: 1px solid var(--accent); display: block; height: 3.35rem; width: 0.42rem; }
.encoder span:nth-child(2) { height: 2.75rem; }
.encoder span:nth-child(3) { height: 2.15rem; }
.encoder span:nth-child(4) { height: 1.55rem; }
.vector { display: grid; gap: 0.12rem; grid-template-columns: repeat(2, 0.35rem); }
.vector i { background: var(--accent); border-radius: 0.08rem; height: 0.35rem; }
.unet { align-items: end; display: grid; grid-template-columns: 1fr 0.65rem 1fr; height: 4.6rem; position: relative; width: 7.7rem; }
.arm { align-items: center; display: flex; gap: 0.18rem; height: 100%; }
.arm.left { justify-content: end; padding-right: 0.18rem; }
.arm.right { justify-content: start; padding-left: 0.18rem; }
.arm i { background: color-mix(in srgb, var(--accent) 65%, #182333); border: 1px solid var(--accent); display: block; width: 0.55rem; }
.arm.left i:nth-child(1), .arm.right i:nth-child(3) { height: 3.8rem; }
.arm.left i:nth-child(2), .arm.right i:nth-child(2) { height: 2.9rem; }
.arm.left i:nth-child(3), .arm.right i:nth-child(1) { height: 2rem; }
.bottleneck { align-self: center; background: #ef8354; border-radius: 0.12rem; height: 1.15rem; width: 0.65rem; }
.skip { border-top: 1px dashed #ef8354; left: 1.61rem; position: absolute; right: 1.61rem; top: 0.4rem; }
.skip-two { left: 2.34rem; right: 2.34rem; top: 0.85rem; }
.mask { background: #182333; }
.mask .object { background: #63d8b0; }
.network-card p { color: var(--text); font-size: 0.58rem; line-height: 1.3; margin: 0.42rem 0 0.3rem; text-align: center; }
.output-label { background: color-mix(in srgb, var(--accent) 12%, transparent); border-radius: 0.3rem; color: var(--accent); font-size: 0.56rem; font-weight: 750; padding: 0.22rem; text-align: center; }
figcaption { color: var(--muted); font-size: 0.47rem; line-height: 1.2; text-align: center; }
</style>
