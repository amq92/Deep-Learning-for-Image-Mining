<script setup lang="ts">
import MathTex from './MathTex.vue'

const heatmapCells = Array.from({ length: 25 }, (_, index) => index)
const vectorCells = Array.from({ length: 15 }, (_, index) => index)
const inputProjectionCells = Array.from({ length: 20 }, (_, index) => index)
const learnedProjectionCells = Array.from({ length: 12 }, (_, index) => index)
const queryProjectionCells = Array.from({ length: 15 }, (_, index) => index)
</script>

<template>
  <figure class="attention-map" aria-label="Self-attention turns query-key comparisons into attention weights, then mixes values">
    <section class="aggregation">
      <div>
        <span class="step">1 · Learned projection matrices</span>
                <MathTex formula="\begin{aligned} Q&amp;=X\textcolor{#7651b5}{\mathbf{W_Q}}\\[-0.45em] K&amp;=X\textcolor{#7651b5}{\mathbf{W_K}}\\[-0.45em] V&amp;=X\textcolor{#7651b5}{\mathbf{W_V}} \end{aligned}" display />
      </div>
      <div class="product" aria-label="attention weights multiply values to produce updated tokens">
        <div class="product-matrix"><span>input tokens <MathTex formula="X" /></span><div class="projection-grid input-projection"><i v-for="cell in inputProjectionCells" :key="`input-${cell}`" /></div></div>
        <b class="product-symbol">×</b>
        <div class="product-matrix"><span>learned matrix <MathTex formula="\mathbf{W_Q}" /></span><div class="projection-grid learned-projection"><i v-for="cell in learnedProjectionCells" :key="`learned-${cell}`" /></div></div>
        <b class="product-symbol">=</b>
        <div class="product-matrix"><span>queries <MathTex formula="Q" /></span><div class="projection-grid query-projection"><i v-for="cell in queryProjectionCells" :key="`query-${cell}`" /></div></div>
      </div>
      <p>Each input token is projected into a query, a key, and a value using three learned matrices.</p>
    </section>

    <section class="heatmap-flow">
      <div class="map-stage score-stage">
        <span class="step">2 · Raw similarity scores</span>
        <div class="equation-and-map">
          <MathTex formula="S=\frac{QK^\top}{\sqrt{d_k}}" display />
          <div class="heatmap-shell">
            <span class="axis top">key token →</span>
            <span class="axis left">query token →</span>
            <div class="heatmap scores" aria-label="raw query key similarity heatmap"><i v-for="cell in heatmapCells" :key="`score-${cell}`" /></div>
          </div>
        </div>
        <p>Value in <MathTex formula="(i,j)" />: how strongly token <MathTex formula="i" /> matches token <MathTex formula="j" />.</p>
      </div>

      <div class="map-stage weights-stage">
        <span class="step">3 · Attention weights</span>
        <div class="equation-and-map">
          <MathTex formula="A=\operatorname{softmax}(S)" display />
          <div class="heatmap-shell">
            <span class="axis top">value token →</span>
            <span class="axis left">query token →</span>
            <div class="heatmap weights" aria-label="row normalized attention weight heatmap"><i v-for="cell in heatmapCells" :key="`weight-${cell}`" /></div>
          </div>
        </div>
        <p>Every row becomes a distribution: each row sums to 1.</p>
      </div>
    </section>

    <section class="aggregation">
      <div>
        <span class="step">4 · Weighted value aggregation</span>
        <MathTex formula="Z=AV" display />
      </div>
      <div class="product" aria-label="attention weights multiply values to produce updated tokens">
        <div class="product-matrix"><span>attention weights <MathTex formula="A" /></span><div class="mini-map weights"><i v-for="cell in heatmapCells" :key="`mini-${cell}`" /></div></div>
        <b class="product-symbol">×</b>
        <div class="product-matrix"><span>value vectors <MathTex formula="V" /></span><div class="vectors values"><i v-for="cell in vectorCells" :key="`value-${cell}`" /></div></div>
        <b class="product-symbol">=</b>
        <div class="product-matrix"><span>self-attention output <MathTex formula="Z" /></span><div class="vectors outputs"><i v-for="cell in vectorCells" :key="`output-${cell}`" /></div></div>
      </div>
      <p>Each row of <MathTex formula="Z" /> is a weighted combination of the value vectors.</p>
    </section>

  </figure>
</template>

<style scoped>
.attention-map { display: grid; gap: 0.6rem; margin: 0; width: 100%; }
.projection, .aggregation { align-items: center; background: var(--surface); border: 1px solid var(--line); border-radius: 0.7rem; display: grid; grid-template-columns: 1.2fr 0.8fr; padding: 0.48rem 0.65rem; }
.projection-equations { display: grid; gap: 0; margin-left: -0.04rem; margin-top: 0.08rem; transform: translateX(-0.3rem); }.projection-equations :deep(.math-tex.display) { line-height: 1; text-align: left; }.projection-equations :deep(.katex-display) { margin: 0; }.projection-product { align-items: stretch; display: flex; gap: 0.42rem; justify-content: center; text-align: center; }.projection-product .product-matrix { grid-template-rows: 1rem auto; }.projection-product .product-symbol { transform: translateY(0.4rem); }.projection-grid { display: grid; gap: 0.08rem; justify-content: center; }.projection-grid i { border-radius: 0.03rem; display: block; height: 0.3rem; width: 0.3rem; }.input-projection { grid-template-columns: repeat(4, 0.3rem); }.input-projection i { background: var(--secondary); }.learned-projection { grid-template-columns: repeat(3, 0.3rem); }.learned-projection i { background: #7651b5; }.query-projection { grid-template-columns: repeat(3, 0.3rem); }.query-projection i { background: var(--accent); }
.qkv-matrices { display: grid; gap: 0.3rem; grid-template-columns: repeat(3, 1fr); }
.qkv-card { border: 2px solid var(--line); border-radius: 0.7rem; padding: 0.28rem 0.2rem 0.34rem; text-align: center; }.qkv-card > span { color: var(--muted); display: block; font-size: 0.42rem; font-weight: 800; text-transform: uppercase; }.qkv-card :deep(.katex) { font-size: 0.88rem; }.qkv-card.query-card { border-color: var(--accent); }.qkv-card.key-card { border-color: var(--secondary); }.qkv-card.value-card { border-color: #b98827; }
.qkv-grid { display: grid; gap: 0.08rem; grid-template-columns: repeat(2, 0.25rem); justify-content: center; margin-top: 0.16rem; }.qkv-grid i { background: var(--accent); border-radius: 0.03rem; display: block; height: 0.25rem; width: 0.25rem; }.key-card .qkv-grid i { background: var(--secondary); }.value-card .qkv-grid i { background: #b98827; }
.step { color: var(--accent); display: block; font-size: 0.48rem; font-weight: 800; letter-spacing: 0.035em; text-transform: uppercase; }
.projection :deep(.katex), .aggregation :deep(.katex) { font-size: 0.84rem; }
p { color: var(--muted); font-size: 0.51rem; line-height: 1.28; margin: 0; }
.heatmap-flow { align-items: center; display: grid; gap: 0.35rem; grid-template-columns: 1fr 1fr; }
.map-stage { background: var(--surface); border: 1px solid var(--line); border-radius: 0.75rem; padding: 0.45rem 0.65rem; text-align: center; }
.map-stage .step { text-align: left; }
.map-stage :deep(.katex) { font-size: 0.84rem; }
.equation-and-map { align-items: center; display: flex; gap: 2.5rem; justify-content: center; margin: 0.35rem 0; }
.equation-and-map :deep(.math-tex.display) { min-width: 3.9rem; }
.heatmap-shell { display: inline-block; margin: 0; padding: 0.35rem 0 0 0.43rem; position: relative; }
.heatmap, .mini-map { display: grid; gap: 0.12rem; grid-template-columns: repeat(5, 0.66rem); }
.heatmap i, .mini-map i { border-radius: 0.06rem; display: block; height: 0.66rem; width: 0.66rem; }
.scores i { background: #fae8e9; }.scores i:nth-child(5n+1), .scores i:nth-child(8n), .scores i:nth-child(11n) { background: #e5485c; }.scores i:nth-child(3n) { background: #f3a8b1; }
.weights i { background: #dceff0; }.weights i:nth-child(5n+1), .weights i:nth-child(8n), .weights i:nth-child(11n) { background: #267a86; }.weights i:nth-child(3n) { background: #83c4c9; }
.axis { color: var(--muted); font-size: 0.4rem; font-weight: 700; position: absolute; }.axis.top { right: 0; top: -0.22rem; }.axis.left { left: -0.14rem; top: 50%; transform: rotate(-90deg) translateX(-50%); transform-origin: left top; }
.arrow { color: var(--accent); font-size: 1.1rem; font-weight: 800; }
.aggregation { grid-template-columns: 0.78fr 1.22fr; }.product { align-items: stretch; display: flex; gap: 0.55rem; justify-content: center; text-align: center; }.product-matrix { display: grid; grid-template-rows: 1.1rem auto; justify-items: center; }.product-matrix > span { align-items: baseline; color: var(--muted); display: flex; font-size: 0.45rem; font-weight: 700; gap: 0.08rem; justify-content: center; line-height: 1; }.product-matrix > span :deep(.katex) { font-size: 0.65rem; }.product-symbol { align-self: center; color: var(--accent); font-size: 1.05rem; line-height: 1; margin: 0; transform: translateY(0.55rem); }.mini-map { gap: 0.09rem; grid-template-columns: repeat(5, 0.4rem); }.mini-map i { height: 0.4rem; width: 0.4rem; }.vectors { display: grid; gap: 0.09rem; grid-template-columns: repeat(3, 0.4rem); }.vectors i { background: #b98827; border-radius: 0.04rem; display: block; height: 0.4rem; width: 0.4rem; }.outputs i { background: var(--accent); }
.aggregation p { grid-column: 1 / -1; margin-top: 0.25rem; text-align: center; }figcaption { color: var(--muted); font-size: 0.48rem; text-align: center; }
</style>
