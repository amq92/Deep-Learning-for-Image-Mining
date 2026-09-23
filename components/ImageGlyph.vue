<script setup lang="ts">
defineProps<{ variant?: 'image' | 'grid' | 'vector' | 'results'; label?: string }>()
</script>

<template>
  <div class="glyph" :class="variant || 'image'">
    <template v-if="variant === 'grid'">
      <span v-for="n in 12" :key="n" :style="{ opacity: 0.35 + (n % 5) * 0.12 }" />
    </template>
    <template v-else-if="variant === 'vector'">
      <i v-for="n in 9" :key="n" :style="{ height: `${18 + ((n * 13) % 44)}%` }" />
    </template>
    <template v-else-if="variant === 'results'">
      <b v-for="n in 6" :key="n" :class="`r${n}`" />
    </template>
    <template v-else>
      <div class="sun" />
      <div class="mountain back" />
      <div class="mountain front" />
    </template>
    <em v-if="label">{{ label }}</em>
  </div>
</template>

<style scoped>
.glyph { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; height: 112px; overflow: hidden; position: relative; width: 150px; }
.glyph em { bottom: 7px; color: var(--muted); font-size: 10px; font-style: normal; font-weight: 700; left: 0; position: absolute; right: 0; text-align: center; }
.sun { background: var(--accent); border-radius: 50%; height: 25px; left: 92px; position: absolute; top: 20px; width: 25px; }
.mountain { bottom: 22px; clip-path: polygon(50% 0, 100% 100%, 0 100%); position: absolute; }
.mountain.back { background: var(--secondary-soft); height: 55px; left: 45px; width: 90px; }
.mountain.front { background: var(--secondary); height: 45px; left: 12px; width: 82px; }
.grid { display: grid; gap: 3px; grid-template-columns: repeat(4, 1fr); padding: 13px; }
.grid span { background: var(--secondary); border-radius: 3px; }
.vector { align-items: center; display: flex; gap: 5px; justify-content: center; padding: 18px; }
.vector i { background: var(--accent); border-radius: 3px 3px 0 0; display: block; width: 7px; }
.results { display: grid; gap: 5px; grid-template-columns: repeat(3, 1fr); padding: 11px; }
.results b { background: var(--secondary-soft); border: 1px solid var(--secondary); border-radius: 4px; }
.results .r2, .results .r5 { background: var(--accent-soft); border-color: var(--accent); }
</style>
