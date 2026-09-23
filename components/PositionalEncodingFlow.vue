<script setup lang="ts">
import MathTex from './MathTex.vue'

const patchCells = Array.from({ length: 16 }, (_, index) => index)
const vectorCells = Array.from({ length: 5 }, (_, index) => index)
const positionTableCells = Array.from({ length: 20 }, (_, index) => index)
</script>

<template>
  <figure class="position-flow" aria-label="A patch is projected with shared parameters, then combined with the learned vector for its grid location">
    <section class="content-route">
      <span class="step">1 · Patch content embedding</span>
      <div class="diagram-node patch-node">
        <span class="node-label">image patch <MathTex formula="x_i" /></span>
        <div class="patch-grid"><i v-for="cell in patchCells" :key="cell" /></div>
      </div>
      <div class="projection-box">
        <span>shared learned linear projection</span>
        <MathTex formula="e_i=x_iE+b" />
        <div class="vector content-vector"><i v-for="cell in vectorCells" :key="cell" /></div>
      </div>
    </section>

    <section class="position-route">
      <span class="step">2 · Position embedding</span>
      <div class="diagram-node location-node">
        <span class="node-label">patch index</span>
        <span class="coordinates"><MathTex formula="i" /></span>
      </div>
      <div class="position-table-node">
        <span class="node-label">learned position table <MathTex formula="P" /></span>
        <div class="position-table" aria-label="Learned position table P with selected row i">
          <i v-for="cell in positionTableCells" :key="cell" :class="{ selected: cell >= 10 && cell < 15 }" />
        </div>
      </div>
      <div class="selected-position-node">
        <span class="node-label">selected row <MathTex formula="p_i" /></span>
        <div class="vector position-vector"><i v-for="cell in vectorCells" :key="cell" /></div>
      </div>
    </section>

    <section class="combination">
      <span class="step">3 · Position-aware input token</span>
      <div class="token-operation">
        <div class="equation"><MathTex formula="t_i=e_i+p_i" display /></div>
        <div class="vector-pair">
          <div class="vector content-vector"><i v-for="cell in vectorCells" :key="`c-${cell}`" /></div>
          <b>+</b>
          <div class="vector position-vector"><i v-for="cell in vectorCells" :key="`p-${cell}`" /></div>
          <b>=</b>
          <div class="vector token-vector"><i v-for="cell in vectorCells" :key="`t-${cell}`" /></div>
        </div>
      </div>
    </section>

  </figure>
</template>

<style scoped>
.position-flow { display: grid; gap: 0.45rem; grid-template-columns: 1fr; margin: 0 auto; width: 100%; }
.content-route, .position-route { align-items: start; background: var(--surface); border: 1px solid var(--line); border-radius: 0.7rem; display: grid; gap: 0.68rem 0.75rem; min-height: 4.6rem; padding: 0.65rem 0.75rem; }.content-route { grid-template-columns: 1fr 1.15fr; }.position-route { grid-template-columns: 0.7fr 1.3fr 0.8fr; }.content-route .step, .position-route .step { grid-column: 1 / -1; }
.step { color: var(--accent); display: block; font-size: 0.54rem; font-weight: 800; letter-spacing: 0.035em; text-align: left; text-transform: uppercase; }
.diagram-node { display: grid; gap: 0.2rem; justify-items: center; text-align: center; }
.node-label, .token-label { align-items: baseline; color: var(--muted); display: flex; font-size: 0.52rem; font-weight: 700; gap: 0.1rem; justify-content: center; line-height: 1.12; }
.node-label :deep(.katex), .token-label :deep(.katex) { font-size: 0.72rem; }
.patch-grid { display: grid; gap: 0.07rem; grid-template-columns: repeat(4, 0.34rem); }
.patch-grid i { background: #d9e5e7; border-radius: 0.03rem; display: block; height: 0.34rem; width: 0.34rem; }
.patch-grid i.selected { background: var(--accent); box-shadow: 0 0 0 1px var(--accent); }
.coordinates { align-items: baseline; color: var(--muted); display: flex; font-size: 0.62rem; font-weight: 700; gap: 0.06rem; padding: 0.37rem 0.45rem; }
.coordinates :deep(.katex) { font-size: 0.72rem; }
.projection-box { align-items: center; align-self: stretch; color: var(--muted); display: grid; font-size: 0.52rem; font-weight: 700; grid-template-rows: repeat(3, 1fr); justify-items: center; line-height: 1.22; padding: 0; text-align: center; }
.projection-box :deep(.katex) { font-size: 0.82rem; margin-top: 0.08rem; }.position-table-node, .selected-position-node { align-items: center; display: grid; gap: 0.26rem; justify-items: center; }.position-table { display: grid; gap: 0.07rem; grid-template-columns: repeat(5, 0.34rem); }.position-table i { background: #d9e5e7; border-radius: 0.03rem; display: block; height: 0.34rem; width: 0.34rem; }.position-table i.selected { background: var(--secondary); box-shadow: 0 0 0 1px var(--secondary); }.arrow, .vector-pair > b { color: var(--accent); font-size: 0.95rem; line-height: 1; text-align: center; }
.vector { display: grid; gap: 0.08rem; grid-template-columns: repeat(5, 0.34rem); }.vector i { border-radius: 0.03rem; display: block; height: 0.34rem; width: 0.34rem; }.content-vector i { background: #b98827; }.position-vector i { background: var(--secondary); }.token-vector i { background: var(--accent); }
.combination { align-items: center; background: var(--surface); border: 1px solid var(--line); border-radius: 0.7rem; display: grid; gap: 0.28rem; grid-template-rows: auto 1fr; min-height: 4.6rem; padding: 0.42rem 0.6rem; text-align: center; }.combination .step { align-self: start; }.token-operation { align-self: center; display: grid; gap: 0.35rem; justify-items: center; }.equation :deep(.katex) { font-size: 0.82rem; }.equation :deep(.katex-display) { margin: 0; }.vector-pair { align-items: center; display: flex; gap: 0.3rem; justify-content: center; }.token-label { justify-self: start; }
figcaption { color: var(--muted); font-size: 0.48rem; text-align: center; }
</style>
