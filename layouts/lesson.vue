<script setup lang="ts">
withDefaults(defineProps<{
  ratio?: 'text' | 'balanced' | 'visual' | 'stacked'
}>(), {
  ratio: 'text',
})
</script>

<template>
  <div class="slidev-layout lesson-layout" :class="`lesson-ratio-${ratio}`">
    <header class="lesson-title">
      <slot name="title" />
    </header>
    <main class="lesson-body">
      <section class="lesson-text">
        <slot name="text" />
      </section>
      <aside class="lesson-visual">
        <slot name="visual" />
      </aside>
    </main>
  </div>
</template>
