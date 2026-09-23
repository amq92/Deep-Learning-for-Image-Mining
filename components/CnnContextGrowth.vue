<script setup>
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const { clicks } = useNav()
const step = computed(() => Math.min(clicks.value, 3))
const stages = [
  { label: 'Layer 1', size: 16 },
  { label: 'Layer 3', size: 34 },
  { label: 'Layer 6', size: 58 },
  { label: 'Layer 10', size: 82 },
]
</script>

<template>
  <figure class="context-growth" aria-label="A focal image region gains a larger receptive field as a convolutional network becomes deeper">
    <span v-for="index in 3" :key="index" v-click class="click-trigger" aria-hidden="true"></span>

    <header>
      <strong>One output position</strong>
      <span>Its input context grows through depth</span>
    </header>

    <div class="scene" :class="`at-step-${step}`">
      <div class="hills hill-a"></div>
      <div class="hills hill-b"></div>
      <div class="road"></div>
      <div class="tree tree-a"></div>
      <div class="tree tree-b"></div>
      <div class="house"></div>
      <div class="person"><i></i><b></b></div>
      <div
        v-for="(stage, index) in stages"
        :key="stage.label"
        class="field"
        :class="[`field-${index}`, { visible: index <= step }]"
        :style="{ '--size': `${stage.size}%`, '--index': index }"
      ></div>
    </div>

    <div class="stage-labels">
      <span v-for="(stage, index) in stages" :key="stage.label" :class="{ active: index === step, seen: index <= step }">{{ stage.label }}</span>
    </div>

    <figcaption>More layers let a CNN combine information from a wider part of the image</figcaption>
  </figure>
</template>

<style scoped>
.context-growth { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; display: grid; gap: 0.55rem; margin: 0; overflow: hidden; padding: 0.8rem; position: relative; width: 100%; }
.click-trigger { height: 0; position: absolute; width: 0; }
header { align-items: baseline; display: flex; justify-content: space-between; }
header strong { color: var(--accent); font-size: 0.67rem; font-weight: 800; letter-spacing: 0.06em; text-transform: uppercase; }
header span { color: var(--muted); font-size: 0.64rem; font-weight: 600; }
.scene { background: linear-gradient(180deg, #b9dce1 0 45%, #d9e5bd 45% 66%, #c2d08c 66%); border: 1px solid color-mix(in srgb, var(--line) 75%, transparent); height: 13rem; overflow: hidden; position: relative; }
.hills { background: #83ae82; border-radius: 50% 50% 0 0; bottom: 33%; opacity: 0.9; position: absolute; }
.hill-a { height: 7rem; left: -8%; width: 58%; }
.hill-b { height: 6rem; right: -13%; width: 62%; }
.road { background: #c8b99c; bottom: -22%; height: 52%; left: 0; position: absolute; transform: skewY(-8deg); width: 100%; }
.tree { background: #47775b; border-radius: 50%; height: 2.6rem; position: absolute; top: 35%; width: 2.3rem; }
.tree::after { background: #745440; bottom: -1.4rem; content: ''; height: 1.8rem; left: 46%; position: absolute; width: 0.32rem; }
.tree-a { left: 14%; }.tree-b { right: 14%; top: 40%; }
.house { background: #e8c071; height: 2.7rem; position: absolute; right: 28%; top: 42%; width: 3.3rem; }
.house::before { border-bottom: 1.5rem solid #b34c43; border-left: 2rem solid transparent; border-right: 2rem solid transparent; content: ''; left: -0.34rem; position: absolute; top: -1.42rem; }
.person { bottom: 24%; height: 2.3rem; left: 47%; position: absolute; width: 1.1rem; z-index: 4; }
.person i { background: #24211f; border-radius: 50%; display: block; height: 0.65rem; margin: auto; width: 0.65rem; }
.person b { background: #df2439; display: block; height: 1.5rem; margin: 0.1rem auto 0; width: 0.72rem; }
.field { border: 3px solid var(--accent); box-shadow: 0 0 0 999px rgba(30, 35, 30, 0.015); left: 52.5%; opacity: 0; position: absolute; top: 66%; transform: translate(-50%, -50%); transition: opacity 180ms ease; width: var(--size); aspect-ratio: 1; z-index: 3; }
.field.visible { opacity: 1; }
.field-1 { border-color: #267a86; }.field-2 { border-color: #d89517; }.field-3 { border-color: #7256a7; }
.stage-labels { display: grid; gap: 0.25rem; grid-template-columns: repeat(4, 1fr); }
.stage-labels span { border: 1px solid var(--line); border-radius: 999px; color: var(--muted); font-size: 0.55rem; font-weight: 700; padding: 0.18rem 0.25rem; text-align: center; }
.stage-labels span.seen { border-color: var(--accent); color: var(--accent); }.stage-labels span.active { background: var(--accent); color: white; }
figcaption { color: var(--muted); font-size: 0.6rem; text-align: center; }
</style>
