<script setup lang="ts">
defineProps<{
  kind: 'neuron' | 'mlp' | 'training' | 'deep'
}>()
</script>

<template>
  <div class="foundation-visual">
    <svg v-if="kind === 'neuron'" viewBox="0 0 520 300" role="img" aria-label="Two inputs are weighted, summed with a bias, and passed through an activation">
      <g class="input-group">
        <circle cx="48" cy="82" r="27" />
        <circle cx="48" cy="218" r="27" />
        <text x="48" y="88">x₁</text>
        <text x="48" y="224">x₂</text>
      </g>
      <g class="flow">
        <path d="M75 82 L187 130" />
        <path d="M75 218 L187 170" />
      </g>
      <g class="weight-labels">
        <rect x="112" y="91" width="54" height="30" rx="9" />
        <rect x="112" y="179" width="54" height="30" rx="9" />
        <text x="139" y="112">w₁</text>
        <text x="139" y="200">w₂</text>
      </g>
      <g class="node emphasis">
        <circle cx="220" cy="150" r="48" />
        <text x="220" y="144">Σ</text>
        <text class="small" x="220" y="168">+ bias</text>
      </g>
      <path class="flow" d="M268 150 L340 150" />
      <g class="node accent">
        <rect x="340" y="112" width="92" height="76" rx="18" />
        <text x="386" y="145">φ(z)</text>
        <text class="small" x="386" y="168">activation</text>
      </g>
      <path class="flow" d="M432 150 L490 150" />
      <text class="output" x="500" y="156">a</text>
      <text class="equation neuron-equation" x="260" y="260">z = wᵀx + b</text>
      <text class="equation neuron-equation" x="260" y="291">a = φ(z)</text>
    </svg>

    <svg v-else-if="kind === 'mlp'" viewBox="0 0 560 310" role="img" aria-label="A multilayer perceptron applies affine transformations and nonlinear activations in a forward pass">
      <defs>
        <marker id="arrow-mlp" markerHeight="8" markerWidth="8" orient="auto" refX="7" refY="4"><path d="M0 0 L8 4 L0 8 Z" /></marker>
      </defs>
      <g class="layer input-layer">
        <circle v-for="y in [110, 210]" :key="`i${y}`" cx="55" :cy="y" r="21" />
        <text x="55" y="116">x₁</text>
        <text x="55" y="216">x₂</text>
      </g>
      <g class="connections">
        <path v-for="(p, i) in ['M76 110 L194 75','M76 110 L194 155','M76 110 L194 235','M76 210 L194 75','M76 210 L194 155','M76 210 L194 235','M236 75 L394 115','M236 75 L394 195','M236 155 L394 115','M236 155 L394 195','M236 235 L394 115','M236 235 L394 195']" :key="i" :d="p" />
      </g>
      <g class="layer hidden-layer">
        <circle v-for="y in [75, 155, 235]" :key="`h${y}`" cx="215" :cy="y" r="21" />
        <text v-for="y in [75, 155, 235]" :key="`phi${y}`" x="215" :y="y + 6">φ</text>
      </g>
      <g class="relu-mini">
        <rect x="269" y="113" width="92" height="72" rx="12" />
        <path class="axes" d="M283 166 L347 166 M305 175 L305 126" />
        <path class="curve" d="M283 166 L305 166 L343 130" />
        <text class="small" x="315" y="201">ReLU</text>
      </g>
      <g class="layer output-layer">
        <circle v-for="y in [115, 195]" :key="`o${y}`" cx="415" :cy="y" r="21" />
      </g>
      <g class="probabilities">
        <rect x="469" y="94" width="70" height="42" rx="10" />
        <rect x="469" y="174" width="70" height="42" rx="10" />
        <text x="504" y="120">0.82</text>
        <text x="504" y="200">0.18</text>
      </g>
      <path class="flow" d="M436 115 L469 115 M436 195 L469 195" />
      <text class="label" x="55" y="36">2 INPUTS</text>
      <text class="label" x="215" y="36">FULLY CONNECTED + RELU</text>
      <text class="label" x="415" y="36">OUTPUT SCORES</text>
      <text class="label" x="504" y="68">SOFTMAX</text>
      <text class="equation" x="280" y="302">a⁽ˡ⁾ = φ(W⁽ˡ⁾a⁽ˡ⁻¹⁾ + b⁽ˡ⁾)</text>
    </svg>

    <figure v-else-if="kind === 'training'" class="landscape-comparison" aria-label="Loss-landscape comparison for VGG, ResNet, and DenseNet architectures">
      <div class="landscape-grid">
        <div class="landscape-panel">
          <strong>VGG-56</strong>
          <img src="/images/loss-landscapes/vgg-56.png" alt="Loss landscape for VGG-56" />
        </div>
        <div class="landscape-panel">
          <strong>VGG-110</strong>
          <img src="/images/loss-landscapes/vgg-110.png" alt="Loss landscape for VGG-110" />
        </div>
        <div class="landscape-panel">
          <strong>ResNet-56</strong>
          <img src="/images/loss-landscapes/resnet-56.png" alt="Loss landscape for ResNet-56" />
        </div>
        <div class="landscape-panel">
          <strong>DenseNet-121</strong>
          <img src="/images/loss-landscapes/densenet-121.png" alt="Loss landscape for DenseNet-121" />
        </div>
      </div>
      <figcaption>
        Source: Li et al., <em>Visualizing the Loss Landscape of Neural Nets</em>, NeurIPS 2018 ·
        <a href="https://www.cs.umd.edu/~tomg/projects/landscapes/">original project</a>
      </figcaption>
    </figure>

    <div v-else class="deep-evidence">
      <figure v-click-hide="1" class="residual-comparison" aria-label="A plain stack is compared with a residual stack that adds a skip connection">
        <div class="network-path">
          <strong>Plain stack</strong>
          <div class="path-diagram">
            <span class="node-pill">x</span><span class="arrow">→</span><span class="layer-box">layer</span><span class="arrow">→</span><span class="layer-box">layer</span><span class="arrow">→</span><span class="node-pill">F(x)</span>
          </div>
          <small>Must learn the full transformation</small>
        </div>
        <div class="network-path">
          <strong>Residual stack</strong>
          <svg class="residual-svg" viewBox="0 0 500 116" role="img" aria-label="The input follows both a learned two-layer path and an identity shortcut before addition">
            <defs>
              <marker id="residual-arrow" markerHeight="7" markerWidth="7" orient="auto" refX="6" refY="3.5">
                <path d="M0 0 L7 3.5 L0 7 Z" />
              </marker>
            </defs>
            <g class="residual-flow">
              <path d="M65 82 H118" />
              <path d="M196 82 H230" />
              <path d="M308 82 H350" />
              <path d="M382 82 H432" />
              <path class="shortcut" d="M52 82 V26 H366 V62" />
            </g>
            <g class="residual-nodes">
              <rect x="20" y="63" width="64" height="38" rx="10" />
              <rect class="learned" x="118" y="63" width="78" height="38" rx="10" />
              <rect class="learned" x="230" y="63" width="78" height="38" rx="10" />
              <circle class="sum" cx="366" cy="82" r="17" />
              <rect x="432" y="63" width="62" height="38" rx="10" />
            </g>
            <g class="residual-labels">
              <text x="52" y="88">x</text>
              <text x="157" y="88">layer</text>
              <text x="269" y="88">layer</text>
              <text x="366" y="88">+</text>
              <text x="463" y="88">F(x)+x</text>
              <text class="shortcut-label" x="225" y="18">identity shortcut</text>
            </g>
          </svg>
          <small>Learns a residual change around the identity</small>
        </div>
      </figure>

      <figure v-click="1" class="resnet-evidence" aria-label="Vertically stacked training and validation error curves for plain and residual networks">
        <div class="resnet-plots">
          <img src="/images/resnet/figure-4-plain.png" alt="ImageNet error curves for 18-layer and 34-layer plain networks" />
          <img src="/images/resnet/figure-4-residual.png" alt="ImageNet error curves for 18-layer and 34-layer residual networks" />
        </div>
        <figcaption>
          He et al., <em>Deep Residual Learning for Image Recognition</em>, CVPR 2016
        </figcaption>
      </figure>
    </div>
  </div>
</template>

<style scoped>
.foundation-visual { width: 100%; }
svg { display: block; max-height: 21rem; width: 100%; }
.landscape-comparison { margin: 0; width: 100%; }
.landscape-grid { display: grid; gap: 0.55rem 0.7rem; grid-template-columns: repeat(2, minmax(0, 1fr)); }
.landscape-panel { min-width: 0; text-align: center; }
.landscape-panel strong { color: var(--ink); display: block; font-family: 'DM Sans', sans-serif; font-size: 0.8rem; margin-bottom: 0.15rem; }
.landscape-panel img { aspect-ratio: 1.46; background: #fff; border: 1px solid var(--line); border-radius: 0.4rem; box-sizing: border-box; display: block; object-fit: contain; padding: 0.12rem; width: 100%; }
.landscape-comparison figcaption { color: var(--muted); font-size: 0.52rem; line-height: 1.25; margin-top: 0.45rem; text-align: center; }
.landscape-comparison figcaption a { color: var(--accent); text-decoration: underline; }
.deep-evidence { display: grid; width: 100%; }
.deep-evidence > * { grid-area: 1 / 1; }
.residual-comparison { display: grid; gap: 0.75rem; margin: 0; width: 100%; }
.network-path { background: var(--surface); border: 1px solid var(--line); border-radius: 0.75rem; padding: 0.8rem; text-align: center; }
.network-path strong { color: var(--ink); display: block; font-family: 'DM Sans', sans-serif; font-size: 0.88rem; margin-bottom: 0.65rem; }
.network-path small { color: var(--muted); display: block; font-size: 0.62rem; margin-top: 0.55rem; }
.path-diagram { align-items: center; display: flex; justify-content: center; min-height: 2.35rem; position: relative; }
.node-pill, .layer-box, .sum-node { align-items: center; background: #fff; border: 1.5px solid var(--ink); border-radius: 0.45rem; color: var(--ink); display: inline-flex; font-family: 'DM Sans', sans-serif; font-size: 0.68rem; font-weight: 700; height: 1.85rem; justify-content: center; min-width: 2.1rem; padding: 0 0.4rem; position: relative; z-index: 2; }
.layer-box { background: var(--accent-soft); border-color: var(--accent); }
.sum-node { background: var(--secondary-soft); border-color: var(--secondary); border-radius: 50%; min-width: 1.85rem; padding: 0; }
.arrow { color: var(--muted); font-size: 1rem; margin: 0 0.2rem; position: relative; z-index: 2; }
.residual-svg { height: 6rem; max-height: none; overflow: visible; }
.residual-flow path { fill: none; marker-end: url(#residual-arrow); stroke: var(--muted); stroke-width: 2; }
.residual-flow path.shortcut { stroke: var(--secondary); }
.residual-nodes rect { fill: #fff; stroke: var(--ink); stroke-width: 1.5; }
.residual-nodes rect.learned { fill: var(--accent-soft); stroke: var(--accent); }
.residual-nodes circle.sum { fill: var(--secondary-soft); stroke: var(--secondary); stroke-width: 1.5; }
.residual-labels text { font-size: 13px; }
.residual-labels .shortcut-label { fill: var(--secondary); font-size: 11px; }
.resnet-evidence { align-self: center; margin: 0; width: 100%; }
.resnet-plots { display: grid; gap: 0.12rem; justify-items: center; }
.resnet-plots img { background: #fff; border: 1px solid var(--line); border-radius: 0.55rem; box-sizing: border-box; display: block; height: 12rem; max-width: 100%; object-fit: contain; padding: 0.28rem; width: auto; }
.resnet-evidence figcaption { color: var(--muted); font-size: 0.48rem; line-height: 1.15; margin-top: 0.15rem; text-align: center; }
text { fill: var(--ink); font-family: 'DM Sans', sans-serif; font-size: 16px; font-weight: 700; text-anchor: middle; }
.small { fill: var(--muted); font-size: 12px; font-weight: 600; }
.label { fill: var(--muted); font-size: 11px; letter-spacing: 0.08em; }
.equation { fill: var(--ink); font-family: 'Source Code Pro', monospace; font-size: 20px; font-weight: 700; }
.input-group circle, .layer circle { fill: var(--surface); stroke: var(--ink); stroke-width: 2; }
.weight-labels rect { fill: var(--surface); stroke: var(--line); stroke-width: 1.5; }
.weight-labels text { fill: var(--accent); font-size: 15px; }
.flow, .flow path, path.flow { fill: none; stroke: var(--muted); stroke-linecap: round; stroke-width: 2.2; }
.node circle, .node rect { fill: var(--surface); stroke: var(--line); stroke-width: 2; }
.node.emphasis circle { fill: var(--secondary-soft); stroke: var(--secondary); }
.node.accent rect { fill: var(--accent-soft); stroke: var(--accent); }
.output { text-anchor: start; }
.connections path { fill: none; opacity: 0.52; stroke: var(--line); stroke-width: 1.6; }
.hidden-layer circle { fill: var(--accent-soft); stroke: var(--accent); }
.output-layer circle { fill: var(--secondary-soft); stroke: var(--secondary); }
.probabilities rect, .relu-mini rect { fill: var(--surface); stroke: var(--line); stroke-width: 1.5; }
.relu-mini .axes { fill: none; stroke: var(--muted); stroke-linecap: round; stroke-width: 1.2; }
.relu-mini .curve { fill: none; stroke: var(--accent); stroke-linecap: round; stroke-linejoin: round; stroke-width: 3; }
.probabilities rect:first-child { fill: var(--accent-soft); stroke: var(--accent); }
</style>
