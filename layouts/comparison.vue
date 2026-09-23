<template>
  <div class="slidev-layout comparison-layout">
    <header class="comparison-title">
      <slot name="title" />
    </header>
    <main class="comparison-body">
      <section class="comparison-column">
        <slot name="left" />
      </section>
      <section class="comparison-column">
        <slot name="right" />
      </section>
    </main>
    <footer class="comparison-takeaway">
      <slot name="takeaway" />
    </footer>
  </div>
</template>

<style scoped>
.comparison-layout {
  display: grid;
  grid-template-rows: auto 1fr auto;
}

.comparison-title {
  border-bottom: 3px solid var(--accent);
  margin-bottom: 1.1rem;
  padding-bottom: 0.75rem;
}

.comparison-title :deep(h1) {
  margin: 0;
  max-width: none;
}

.comparison-body {
  display: grid;
  gap: 2.5rem;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  min-height: 0;
}

.comparison-column {
  font-size: 0.95rem;
  line-height: 1.4;
}

.comparison-column :deep(h2) {
  margin: 0 0 0.75rem;
}

.comparison-column :deep(ul) {
  margin: 1rem 0 0;
  padding-left: 1.1rem;
}

.comparison-column :deep(li) {
  margin-bottom: 0.8rem;
}

.comparison-takeaway :deep(blockquote) {
  background: var(--accent-soft);
  border-left: 5px solid var(--accent);
  border-radius: 8px;
  color: var(--accent);
  font-size: 1.05rem;
  font-weight: 700;
  line-height: 1.35;
  margin: 0.75rem 0 0;
  padding: 0.75rem 0.9rem;
}

.comparison-takeaway :deep(blockquote p) {
  color: inherit;
  margin: 0;
}
</style>
