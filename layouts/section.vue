<script setup lang="ts">
defineProps<{
  number?: string | number
}>()
</script>

<template>
  <div class="slidev-layout section-layout">
    <div class="section-number" aria-hidden="true">{{ number }}</div>
    <main class="section-content">
      <div class="section-rule"></div>
      <slot name="title" />
      <div class="section-question">
        <slot name="question" />
      </div>
    </main>
  </div>
</template>

<style scoped>
.section-layout {
  align-items: center;
  display: grid;
  grid-template-columns: 0.7fr 2fr;
  gap: 2.8rem;
  height: 100%;
  overflow: hidden;
  padding: 3.5rem 4.2rem;
  position: relative;
}

.section-number {
  color: var(--accent);
  font-family: 'Manrope', sans-serif;
  font-size: 9rem;
  font-weight: 800;
  letter-spacing: -0.09em;
  line-height: 0.8;
  opacity: 0.16;
  text-align: right;
}

.section-content {
  max-width: 38rem;
}

.section-rule {
  background: var(--accent);
  height: 0.35rem;
  margin-bottom: 1.5rem;
  width: 4rem;
}

.section-content :deep(h1) {
  font-size: 3.25rem;
  line-height: 0.98;
  margin: 0;
}

.section-question {
  border-top: 1px solid var(--line);
  margin-top: 2rem;
  padding-top: 1.2rem;
}

.section-question :deep(p) {
  color: var(--muted);
  font-size: 1.35rem;
  line-height: 1.35;
  margin: 0;
}
</style>
