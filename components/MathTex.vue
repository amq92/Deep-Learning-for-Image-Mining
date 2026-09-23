<script setup lang="ts">
import { renderToString } from 'katex'
import { computed } from 'vue'

const props = defineProps<{
  formula: string
  display?: boolean
}>()

const rendered = computed(() => renderToString(props.formula, {
  displayMode: props.display ?? false,
  throwOnError: false,
  strict: 'warn',
}))
</script>

<template>
  <span class="math-tex" :class="{ display }" v-html="rendered" />
</template>

<style scoped>
.math-tex.display {
  display: block;
  text-align: center;
}
</style>
