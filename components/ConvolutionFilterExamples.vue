<script setup>
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const { clicks } = useNav()
const afterRelu = computed(() => clicks.value > 0)
</script>

<template>
  <figure class="filter-examples" aria-label="Spatial and multichannel kernels produce feature maps before and after ReLU">
    <span v-click class="click-trigger" aria-hidden="true"></span>
    <div class="filter-row">
      <div class="example"><img src="/images/representation/duck-alternate.jpg" alt="Original duck photograph"></div>
      <b>→</b>
      <div class="kernel-card"><strong>Gaussian blur</strong><div class="mini-matrix"><span>1</span><span>2</span><span>1</span><span>2</span><span>4</span><span>2</span><span>1</span><span>2</span><span>1</span></div><small>× 1/16</small></div>
      <b>→</b>
      <div class="example"><img src="/images/representation/duck-blur.jpg" alt="Duck photograph after blur filtering"></div>
    </div>
    <div class="filter-row">
      <div class="example"><img src="/images/representation/duck-alternate.jpg" alt="Original duck photograph"></div>
      <b>→</b>
      <div class="kernel-card gradient-card"><strong>Horizontal gradient</strong><div class="mini-matrix"><span>−1</span><span>0</span><span>1</span><span>−2</span><span>0</span><span>2</span><span>−1</span><span>0</span><span>1</span></div><small>Sobel filter</small></div>
      <b>→</b>
      <div class="example"><img v-if="afterRelu" src="/images/representation/duck-vertical-edges-relu.png" alt="Horizontal Sobel feature map after ReLU"><img v-else src="/images/representation/duck-vertical-edges.png" alt="Signed horizontal Sobel feature map before ReLU"></div>
    </div>
    <div class="filter-row rgb-row">
      <div class="example"><img src="/images/representation/duck-alternate.jpg" alt="RGB duck photograph"></div>
      <b>→</b>
      <div class="kernel-card"><strong>Yellow opponent</strong><div class="rgb-kernel"><div class="slice red-slice"><span>½</span><span>1</span><span>½</span><span>1</span><span>2</span><span>1</span><span>½</span><span>1</span><span>½</span></div><div class="slice green-slice"><span>½</span><span>1</span><span>½</span><span>1</span><span>2</span><span>1</span><span>½</span><span>1</span><span>½</span></div><div class="slice blue-slice"><span>−1</span><span>−2</span><span>−1</span><span>−2</span><span>−4</span><span>−2</span><span>−1</span><span>−2</span><span>−1</span></div></div><small>R · G · B slices</small></div>
      <b>→</b>
      <div class="example"><img v-if="afterRelu" src="/images/representation/duck-yellow-kernel-response-relu.png" alt="Yellow-opponent feature map after ReLU"><img v-else src="/images/representation/duck-yellow-kernel-response.png" alt="Signed yellow-opponent feature map before ReLU"></div>
    </div>
          <div class="step-control"><span :class="{ active: !afterRelu }">1 · output of convolution</span><span :class="{ active: afterRelu }">2 · after ReLU</span></div>
        </figure>
</template>

<style scoped>
.filter-examples { display: grid; gap: 0.58rem; margin: 0; position: relative; width: 100%; }
.click-trigger { height: 0; overflow: hidden; position: absolute; width: 0; }
.filter-row { align-items: center; display: grid; gap: 0.85rem; grid-template-columns: 6.3rem 1.2rem 8rem 1.2rem 6.3rem; justify-content: center; }
.filter-row > b { color: var(--accent); font-size: 1.35rem; }
.example { align-items: center; display: flex; }
.example img { border: 1px solid var(--line); border-radius: 9px; height: 4.35rem; image-rendering: auto; object-fit: cover; width: 6.25rem; }
.kernel-card { align-items: center; display: flex; flex-direction: column; gap: 0.2rem; text-align: center; }
.kernel-card strong { color: var(--accent); font-size: 0.66rem; white-space: nowrap; }
.gradient-card strong { color: #dc2626; }
.kernel-card small { color: var(--muted); font-family: 'Fira Code', monospace; font-size: 0.5rem; }
.mini-matrix { display: grid; gap: 2px; grid-template-columns: repeat(3, 1.15rem); }
.mini-matrix span { align-items: center; background: var(--surface-2); border: 1px solid var(--line); border-radius: 3px; display: flex; font-family: 'Fira Code', monospace; font-size: 0.52rem; height: 1rem; justify-content: center; }
.rgb-kernel { height: 2.15rem; position: relative; width: 5.5rem; }
.slice { display: grid; gap: 1px; grid-template-columns: repeat(3, 0.66rem); position: absolute; }
.slice span { align-items: center; border: 1px solid currentColor; border-radius: 2px; display: flex; font-family: 'Fira Code', monospace; font-size: 0.34rem; height: 0.58rem; justify-content: center; }
.red-slice { color: #dc2626; left: 0; top: 0; }
.green-slice { color: #16a34a; left: 1.65rem; top: 0.28rem; }
.blue-slice { color: #2563eb; left: 3.3rem; top: 0.56rem; }
.red-slice span { background: #fee2e2; }
.green-slice span { background: #dcfce7; }
.blue-slice span { background: #dbeafe; }
.rgb-row { border-top: 1px solid var(--line); padding-top: 0.55rem; }
.step-control { background: var(--surface-2); border-radius: 8px; display: flex; gap: 0.35rem; justify-content: center; margin: 1.05rem auto 0; padding: 0.2rem 0.35rem; }
.step-control span { border: 1px solid transparent; border-radius: 999px; color: var(--muted); font-size: 0.48rem; font-weight: 700; padding: 0.16rem 0.42rem; }
.step-control span.active { background: var(--accent); border-color: var(--accent); color: white; }
</style>
