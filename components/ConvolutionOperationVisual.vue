<script setup>
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const input = [
  [1, 2, 0, 1, 3],
  [0, 1, 3, 2, 1],
  [2, 1, 2, 0, 1],
  [1, 0, 1, 3, 2],
  [3, 2, 0, 1, 0],
]
const kernel = [
  [1, 0, -1],
  [1, 0, -1],
  [1, 0, -1],
]
const positions = [[0, 0], [0, 1]]
const { clicks } = useNav()
const step = computed(() => Math.min(clicks.value, 2))
const position = computed(() => positions[Math.min(step.value, 1)])
const output = computed(() => Array.from({ length: 3 }, (_, row) =>
  Array.from({ length: 3 }, (_, col) =>
    kernel.flatMap((values, u) => values.map((weight, v) => weight * input[row + u][col + v]))
      .reduce((sum, value) => sum + value, 0),
  ),
))
const isPatch = (row, col) => {
  if (step.value === 2) return false
  const [top, left] = position.value
  return row >= top && row < top + 3 && col >= left && col < left + 3
}
</script>

<template>
  <figure class="convolution-visual" aria-label="A kernel slides over an input matrix and produces a feature map">
    <span v-click class="click-trigger" aria-hidden="true"></span>
    <span v-click class="click-trigger" aria-hidden="true"></span>

    <div class="operation">
      <section>
        <h3>Input</h3>
        <div class="matrix input-matrix">
          <template v-for="(row, i) in input" :key="i">
            <span v-for="(value, j) in row" :key="j" :class="{ selected: isPatch(i, j) }">{{ value }}</span>
          </template>
        </div>
      </section>

      <b class="symbol">⊙</b>

      <section>
        <h3>Kernel</h3>
        <div class="matrix kernel-matrix">
          <template v-for="(row, i) in kernel" :key="i">
            <span v-for="(value, j) in row" :key="j">{{ value }}</span>
          </template>
        </div>
      </section>

      <div class="arrow"><b>→</b></div>

      <section>
        <h3>Output</h3>
        <div class="matrix output-matrix">
          <template v-for="(row, i) in output" :key="i">
            <span
              v-for="(value, j) in row"
              :key="j"
              :class="{ active: (step === 0 && i === 0 && j === 0) || (step === 1 && i === 0 && j === 1), complete: step === 2 }"
            >{{ step === 2 || (i === 0 && j <= step) ? value : '·' }}</span>
          </template>
        </div>
      </section>
    </div>

    <div class="step-control">
      <span :class="{ active: step === 0 }">1 · first position</span>
      <span :class="{ active: step === 1 }">2 · slide right</span>
      <span :class="{ active: step === 2 }">3 · complete map</span>
    </div>
  </figure>
</template>

<style scoped>
.convolution-visual { display: grid; gap: 0.65rem; margin: 0; position: relative; width: 100%; }
.click-trigger { height: 0; overflow: hidden; position: absolute; width: 0; }
.operation { align-items: center; display: grid; gap: 0.55rem; grid-template-columns: 6.35rem 1.4rem 3.8rem 1.4rem 3.8rem; justify-content: center; }
section { display: flex; flex-direction: column; align-items: center; text-align: center; }
h3 { color: var(--muted); font-size: 0.59rem; letter-spacing: 0.06em; margin: 0 0 0.3rem; text-transform: uppercase; }
.matrix { display: grid; gap: 2px; }
.matrix span { align-items: center; background: var(--surface-2); border: 1px solid var(--line); border-radius: 3px; display: flex; font-family: 'Fira Code', monospace; font-size: 0.58rem; height: 1.23rem; justify-content: center; width: 1.23rem; }
.input-matrix { grid-template-columns: repeat(5, 1.23rem); }
.kernel-matrix, .output-matrix { grid-template-columns: repeat(3, 1.23rem); }
.input-matrix span.selected { background: var(--accent-soft); border-color: var(--accent); color: var(--accent); font-weight: 800; }
.kernel-matrix span { background: var(--secondary-soft); border-color: var(--secondary); color: var(--secondary); font-weight: 800; }
.output-matrix span.active { background: var(--accent); border-color: var(--accent); color: white; font-weight: 800; }
.output-matrix span.complete { background: var(--accent-soft); border-color: var(--accent); color: var(--accent); font-weight: 800; }
.symbol { align-items: center; align-self: center; color: var(--secondary); display: flex; font-size: 1.15rem; justify-content: center; line-height: 1; transform: translateY(1.23rem); }
.arrow { align-items: center; align-self: center; display: flex; justify-content: center; transform: translateY(1.23rem); }
.arrow b { color: var(--accent); font-size: 1.45rem; line-height: 1; }
.step-control { align-items: center; background: var(--surface-2); border-radius: 8px; display: flex; gap: 0.35rem; justify-content: center; padding: 0.28rem 0.5rem; }
.step-control span { border: 1px solid transparent; border-radius: 999px; color: var(--muted); font-size: 0.52rem; font-weight: 700; padding: 0.2rem 0.48rem; }
.step-control span.active { background: var(--accent); border-color: var(--accent); color: white; }
</style>
