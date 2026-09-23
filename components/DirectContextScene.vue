<script setup>
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $clicks: clicks } = useSlideContext()
const revealScene = computed(() => clicks.value > 0)
const revealRegions = computed(() => clicks.value > 1)
</script>

<template>
  <figure class="direct-context" aria-label="A person gains information from unequal-sized contextual regions in a pedestrian crossing scene">
    <span v-click class="click-trigger" aria-hidden="true"></span>
    <span v-click class="click-trigger" aria-hidden="true"></span>
    <div class="image-frame" :class="{ reveal: revealScene }">
      <img src="/images/vit/barcelona-crosswalk-512x320.jpg" alt="A person crossing a street near a crosswalk and pedestrian signal in Barcelona" />
      <div v-if="revealRegions" class="context-regions">
        <div class="region crosswalk crosswalk-left"><span>crosswalk</span></div>
        <div class="region crosswalk crosswalk-right"><span>crosswalk</span></div>
        <div class="region curb"><span>curb</span></div>
        <div class="region signal"><span>pedestrian signal</span></div>
      </div>
    </div>
    <div class="states"><span :class="{ active: !revealScene }">1 · person alone</span><span :class="{ active: revealScene && !revealRegions }">2 · scene context</span><span :class="{ active: revealRegions }">3 · useful regions</span></div>
    <figcaption>Useful visual context is not naturally divided into equal-sized regions</figcaption>
  </figure>
</template>

<style scoped>
.direct-context { display: grid; gap: 0.5rem; margin: 0; position: relative; width: 100%; }.click-trigger { height: 0; position: absolute; width: 0; }
.image-frame { aspect-ratio: 16 / 10; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; box-shadow: 0 8px 22px var(--shadow); overflow: hidden; position: relative; width: 100%; }
.image-frame img { display: block; height: 100%; object-fit: cover; object-position: center; transform: scale(2.56); transform-origin: 40% 54.7%; transition: transform 500ms ease; width: 100%; }.image-frame.reveal img { transform: scale(1); }
.context-regions { inset: 0; position: absolute; }.region { border: 2px solid var(--accent); position: absolute; }.region span { background: var(--surface); border: 1px solid var(--accent); border-radius: 999px; bottom: calc(100% + 0.2rem); color: var(--accent); font-size: 0.52rem; font-weight: 800; left: 0; padding: 0.1rem 0.28rem; position: absolute; white-space: nowrap; z-index: 1; }.crosswalk { height: 20%; }.crosswalk-left { bottom: 6%; left: 0%; width: 15%; height: 12%; }.crosswalk-right { bottom: 0%; right: 0%; width: 18%; }.curb { bottom: 13%; height: 7%; left: 55%; width: 25%; }.signal { height: 13%; right: 12%; top: 37%; width: 10%; }
.states { display: grid; gap: 0.25rem; grid-template-columns: repeat(3, 1fr); }.states span { border: 1px solid var(--line); border-radius: 999px; color: var(--muted); font-size: 0.54rem; font-weight: 700; padding: 0.2rem; text-align: center; }.states span.active { background: var(--accent); border-color: var(--accent); color: white; } figcaption { color: var(--muted); font-size: 0.6rem; text-align: center; }
</style>
