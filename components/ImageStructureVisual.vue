<template>
  <figure class="comparison" aria-label="Comparison between a fully connected layer and a convolutional layer processing an image">
    <section class="path mlp-path">
      <header><span>Generic MLP</span><strong>Flatten first</strong></header>
      <div class="flow">
        <div class="image-grid pixels" aria-label="Image pixels arranged as a grid"></div>
        <span class="arrow">→</span>
        <div class="flat-vector" aria-label="Flattened pixel vector"><i v-for="n in 12" :key="n"></i></div>
        <span class="arrow">→</span>
        <div class="dense" aria-label="Every input connects independently"><i v-for="n in 4" :key="n"></i></div>
      </div>
      <div class="shape-flow"><span><small>RGB input</small>224 × 224 × 3</span><i>→</i><span><small>1-channel output</small>224 × 224 × 1</span></div>
      <div class="parameter-count dense-count"><strong>7.55 billion weights</strong></div>
    </section>

    <section class="path conv-path">
      <header><span>Convolution</span><strong>Keep the grid</strong></header>
      <div class="flow">
        <div class="image-grid receptive" aria-label="A local window moves across the image"><i></i></div>
        <span class="arrow">→</span>
        <div class="kernel" aria-label="One learned filter is reused"><i v-for="n in 9" :key="n"></i></div>
        <span class="arrow">→</span>
        <div class="image-grid activation" aria-label="Spatial activation map"></div>
      </div>
      <div class="shape-flow"><span><small>RGB input</small>224 × 224 × 3</span><i>→</i><span><small>1-channel output</small>224 × 224 × 1</span></div>
      <div class="parameter-count conv-count"><span>3 × 3 × 3 kernel</span><strong>27 shared weights</strong></div>
    </section>

  </figure>
</template>

<style scoped>
.comparison { display: grid; gap: 0.72rem; margin: 0; width: 100%; }
.path { border: 1px solid var(--line); border-radius: 0.75rem; padding: 0.7rem 0.85rem; }
.mlp-path { background: var(--surface); }
.conv-path { background: var(--accent-soft); border-color: color-mix(in srgb, var(--accent) 35%, var(--line)); }
header { align-items: baseline; display: flex; justify-content: space-between; margin-bottom: 0.48rem; }
header span { color: var(--accent); font-size: 0.65rem; font-weight: 800; letter-spacing: 0.07em; text-transform: uppercase; }
header strong { color: var(--ink); font-size: 0.72rem; }
.flow { align-items: center; display: flex; justify-content: center; min-height: 4.4rem; }
.arrow { color: var(--accent); font-size: 1.25rem; font-weight: 800; margin: 0 0.55rem; }
.image-grid { background-color: #fff; background-image: linear-gradient(var(--line) 1px, transparent 1px), linear-gradient(90deg, var(--line) 1px, transparent 1px); background-size: 0.62rem 0.62rem; border: 1px solid var(--ink); border-radius: 0.25rem; height: 3.7rem; position: relative; width: 3.7rem; }
.pixels { background-color: var(--secondary-soft); }
.receptive { background-color: #fff; }
.receptive i { background: color-mix(in srgb, var(--accent) 48%, transparent); border: 2px solid var(--accent); height: 1.86rem; left: 0.62rem; position: absolute; top: 0.62rem; width: 1.86rem; }
.activation { background-color: var(--surface); background-image: radial-gradient(circle at 42% 42%, var(--accent) 0 0.35rem, transparent 0.4rem), linear-gradient(var(--line) 1px, transparent 1px), linear-gradient(90deg, var(--line) 1px, transparent 1px); background-size: auto, 0.62rem 0.62rem, 0.62rem 0.62rem; }
.flat-vector { display: grid; gap: 0.09rem; grid-template-columns: repeat(2, 0.28rem); }
.flat-vector i { background: var(--secondary); border-radius: 1px; height: 0.28rem; opacity: 0.65; }
.dense { align-items: center; display: flex; gap: 0.3rem; height: 3.7rem; position: relative; }
.dense::before { background: repeating-linear-gradient(73deg, transparent 0 5px, var(--line) 6px 7px); content: ''; inset: 0.35rem -0.25rem; position: absolute; }
.dense i { background: #fff; border: 1px solid var(--ink); border-radius: 50%; height: 0.62rem; position: relative; width: 0.62rem; z-index: 1; }
.kernel { display: grid; gap: 0.08rem; grid-template-columns: repeat(3, 0.55rem); }
.kernel i { background: var(--secondary); border-radius: 2px; height: 0.55rem; opacity: 0.5; }
.kernel i:nth-child(5) { background: var(--accent); opacity: 1; }
.shape-flow { align-items: center; border-top: 1px solid var(--line); display: flex; font-family: 'Fira Code', monospace; font-size: 0.5rem; gap: 0.55rem; justify-content: center; margin-top: 0.55rem; padding-top: 0.42rem; }
.shape-flow span { color: var(--ink); text-align: center; }
.shape-flow small { color: var(--muted); display: block; font-family: 'DM Sans', sans-serif; font-size: 0.4rem; }
.shape-flow i { color: var(--accent); font-style: normal; }
.parameter-count { align-items: center; display: flex; font-family: 'Fira Code', monospace; font-size: 0.5rem; gap: 0.38rem; justify-content: center; margin-top: 0.3rem; }
.parameter-count span { color: var(--muted); }
.parameter-count i { color: var(--accent); font-style: normal; }
.parameter-count strong { border-radius: 999px; margin-left: 0.22rem; padding: 0.18rem 0.4rem; }
.dense-count strong { background: var(--secondary-soft); color: var(--secondary); }
.conv-count strong { background: var(--accent); color: white; }
</style>
