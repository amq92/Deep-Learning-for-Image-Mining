<script setup>
import { computed } from 'vue'

const props = defineProps({ mode: { type: String, default: 'grid' } })
const phase = 2
const cells = Array.from({ length: 160 }, (_, index) => ({ row: Math.floor(index / 16), col: index % 16 }))
const highlighted = cell => (cell.row >= 5 && cell.row <= 9 && cell.col >= 5 && cell.col <= 7) || (cell.row >= 3 && cell.row <= 4 && cell.col >= 12 && cell.col <= 13)
const showGrid = computed(() => true)
</script>

<template>
  <figure class="patch-scene" :class="`mode-${mode}`" aria-label="A street image is divided into a regular sixteen by ten patch grid">
    <div class="image-grid" :class="{ grid: showGrid, positions: mode === 'positions' && phase >= 2 }">
      <img src="/images/vit/barcelona-crosswalk-512x320.jpg" alt="Street scene divided into a sixteen by ten grid of image patches" />
      <div class="patches" aria-hidden="true"><i v-for="cell in cells" :key="`${cell.row}-${cell.col}`" :class="{ highlight: highlighted(cell) }"></i></div>
      <div v-if="mode === 'positions' && phase >= 2" class="position-layer" aria-hidden="true"><i v-for="cell in cells" :key="`p-${cell.row}-${cell.col}`" :class="{ highlight: highlighted(cell) }"><small>p{{ cell.row * 16 + cell.col + 1 }}</small></i></div>
    </div>
    <div v-if="mode === 'grid'" class="caption-strip"><span class="active">160 patches</span></div>
    <div v-else class="caption-strip caption-spacer" aria-hidden="true"><span>placeholder</span></div>
    <figcaption v-if="mode === 'grid'">512 × 320 px image → 16 × 10 patches of 32 × 32 px</figcaption>
    <figcaption v-else class="caption-spacer" aria-hidden="true">placeholder</figcaption>
  </figure>
</template>

<style scoped>
.patch-scene { display: grid; gap: 0.48rem; margin: 0; position: relative; width: 100%; }.click-trigger { height: 0; position: absolute; width: 0; }.image-grid { aspect-ratio: 16 / 10; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; box-shadow: 0 8px 22px var(--shadow); overflow: hidden; position: relative; }.image-grid > img { display: block; height: 100%; object-fit: cover; width: 100%; }.patches, .position-layer { display: grid; grid-template-columns: repeat(16, 1fr); grid-template-rows: repeat(10, 1fr); inset: 0; pointer-events: none; position: absolute; }.patches { opacity: 0; transition: opacity 300ms ease; }.grid .patches { opacity: 1; }.patches i { border-right: 1px solid rgba(255,255,255,0.78); border-bottom: 1px solid rgba(255,255,255,0.78); box-sizing: border-box; }.patches i.highlight { background: rgba(223,36,57,0.25); box-shadow: inset 0 0 0 1px var(--accent); }.position-layer { background: rgba(244,241,235,0.82); opacity: 0; transition: opacity 300ms ease; }.positions .position-layer { opacity: 1; }.position-layer i { align-items: center; border: 1px solid var(--line); box-sizing: border-box; display: flex; justify-content: center; }.position-layer i.highlight { background: var(--accent-soft); border-color: var(--accent); }.position-layer small { color: var(--muted); font-size: 0.28rem; font-weight: 800; }.position-layer i.highlight small { color: var(--accent); }.plus { align-items: center; background: var(--secondary); border: 2px solid var(--surface); border-radius: 50%; color: white; display: flex; font-size: 1rem; font-weight: 800; height: 1.4rem; justify-content: center; left: 50%; position: absolute; top: 50%; transform: translate(-50%, -50%); width: 1.4rem; z-index: 3; }.caption-strip { display: grid; gap: 0.25rem; grid-template-columns: repeat(3, 1fr); }.mode-grid .caption-strip { grid-template-columns: 1fr; justify-items: center; }.mode-grid .caption-strip span { width: calc((100% - 0.5rem) / 3); }.caption-strip span { border: 1px solid var(--line); border-radius: 999px; color: var(--muted); font-size: 0.53rem; font-weight: 700; padding: 0.2rem; text-align: center; }.caption-strip span.active { background: var(--accent); border-color: var(--accent); color: white; }.caption-spacer { visibility: hidden; }.mode-grid .caption-strip span.active { background: #f5dfa0; border-color: #d1a944; color: #5f4700; } figcaption { color: var(--muted); font-size: 0.6rem; text-align: center; } figcaption strong { color: var(--accent); }
</style>
