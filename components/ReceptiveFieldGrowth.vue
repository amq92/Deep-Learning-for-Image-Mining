<template>
  <figure class="rf-growth" aria-label="A deeper activation depends on a growing path through earlier layers and a larger receptive field in the input">
    <section class="concept-block receptive-block">
      <header><strong>Receptive field</strong><span>Context grows through depth</span></header>
      <div class="trace-row">
        <div class="trace-layers">
          <template v-for="(stage, index) in stages" :key="stage.layer">
            <div class="feature-map">
              <span class="support" :style="{ '--support': `${stage.support * traceCell}rem` }"></span>
              <i class="selected"></i>
              <small>Layer {{ stage.layer }}</small>
            </div>
            <b v-if="index < stages.length - 1">→</b>
          </template>
        </div>
      </div>
    </section>

    <section class="concept-block pooling-block">
      <header><strong>Pooling</strong><span>Spatial resolution decreases</span></header>
      <div class="pooling-example" aria-label="A two by two max-pooling operation reduces a four by four feature map to two by two">
      <div class="matrix-block">
        <div class="number-grid input-grid">
          <span
            v-for="(value, index) in poolingInput"
            :key="`input-${index}`"
            :class="`region-${Math.floor(index / 8) * 2 + Math.floor((index % 4) / 2)}`"
          >{{ value }}</span>
        </div>
        <small><strong>Input feature map</strong><span>4 × 4</span></small>
      </div>

      <div class="pool-operator">
        <div class="operator-content">
          <div class="operator-label">
            <strong>2 × 2 max pool</strong>
            <span>stride 2</span>
          </div>
          <b>→</b>
        </div>
      </div>

      <div class="matrix-block">
        <div class="number-grid output-grid">
          <span v-for="(value, index) in poolingOutput" :key="`output-${index}`" :class="`region-${index}`">{{ value }}</span>
        </div>
        <small><strong>Pooled feature map</strong><span>2 × 2</span></small>
      </div>
      </div>
    </section>
  </figure>
</template>

<script setup>
const traceCell = 0.72
const stages = [
  { layer: 1, support: 5 },
  { layer: 2, support: 3 },
  { layer: 3, support: 1 },
]
const poolingInput = [
  1, 3, 2, 4,
  5, 2, 7, 1,
  0, 6, 3, 8,
  4, 2, 1, 5,
]
const poolingOutput = [5, 7, 6, 8]
</script>

<style scoped>
.rf-growth { display: grid; gap: 1.1rem; margin: 0; width: 100%; }
.concept-block { border: 1px solid var(--line); border-radius: 0.75rem; padding: 0.58rem 0.75rem 0.7rem; }
.receptive-block { background: var(--surface); padding-bottom: 1.65rem; }
.pooling-block { background: var(--accent-soft); border-color: color-mix(in srgb, var(--accent) 35%, var(--line)); }
.concept-block header { align-items: baseline; display: flex; justify-content: space-between; margin-bottom: 0.48rem; }
.concept-block header strong { color: var(--accent); font-size: 0.65rem; font-weight: 800; letter-spacing: 0.07em; text-transform: uppercase; }
.concept-block header span { color: var(--ink); font-size: 0.62rem; font-weight: 700; }
.trace-row { display: grid; gap: 0.35rem; }
.trace-layers { align-items: center; display: flex; gap: 0.9rem; justify-content: center; }
.trace-layers > b { color: var(--secondary); font-size: 1.1rem; margin: 0; }
.feature-map { background-color: #fff; background-image: linear-gradient(var(--line) 1px, transparent 1px), linear-gradient(90deg, var(--line) 1px, transparent 1px); background-position: center; background-size: 0.72rem 0.72rem; border: 1px solid var(--ink); box-sizing: content-box; height: 5.04rem; position: relative; width: 5.04rem; }
.feature-map small { bottom: -1.35rem; color: var(--accent); font-size: 0.72rem; font-weight: 800; left: 0; position: absolute; right: 0; text-align: center; }
.support { background: color-mix(in srgb, var(--accent) 23%, transparent); border: 1.5px solid var(--accent); height: var(--support); left: 50%; position: absolute; top: 50%; transform: translate(-50%, -50%); width: var(--support); }
.selected, .output { aspect-ratio: 1 / 1; background: var(--secondary); border: 1.5px solid white; border-radius: 50%; box-shadow: 0 0 0 1px var(--secondary); box-sizing: border-box; height: 0.34rem; left: 50%; padding: 0; position: absolute; top: 50%; transform: translate(-50%, -50%); width: 0.34rem; }
.pooling-example { align-items: center; display: flex; gap: 0.85rem; justify-content: center; }
.matrix-block { align-items: center; display: grid; grid-template-rows: 3.75rem 1.6rem; justify-items: center; }
.matrix-block .number-grid { align-self: center; }
.matrix-block small { align-items: center; align-self: end; margin-top: 0.25rem; color: var(--muted); display: flex; flex-direction: column; font-size: 0.56rem; line-height: 1.15; min-height: 1.35rem; }
.matrix-block small strong { color: var(--ink); font-size: 0.58rem; }
.matrix-block small span { font-family: 'Fira Code', monospace; font-weight: 700; }
.number-grid { display: grid; font-family: 'Fira Code', monospace; font-weight: 700; }
.number-grid span { align-items: center; border-style: solid; border-width: 1px; display: flex; justify-content: center; }
.input-grid { grid-template-columns: repeat(4, 0.92rem); }
.input-grid span { font-size: 0.53rem; height: 0.92rem; }
.output-grid { grid-template-columns: repeat(2, 0.92rem); }
.output-grid span { font-size: 0.62rem; height: 0.92rem; }
.region-0 { background: #dbeafe; border-color: #3b82f6; }
.region-1 { background: #ffedd5; border-color: #f97316; }
.region-2 { background: #dcfce7; border-color: #22c55e; }
.region-3 { background: #f3e8ff; border-color: #a855f7; }
.pool-operator { display: grid; grid-template-rows: 3.75rem 1.6rem; min-width: 4.4rem; }
.operator-content { align-items: center; align-self: center; display: grid; grid-template-columns: auto 1.65rem; }
.operator-label { align-items: center; display: flex; flex-direction: column; }
.pool-operator strong { color: var(--ink); font-size: 0.7rem; }
.pool-operator span { color: var(--muted); font-family: 'Fira Code', monospace; font-size: 0.55rem; }
.pool-operator b { align-self: center; color: var(--secondary); font-size: 1.55rem; line-height: 1; margin-left: 0.2rem; }
</style>
