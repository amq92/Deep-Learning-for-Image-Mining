<template>
  <div class="title-slide">
    <div class="title-slide__dot-field" aria-hidden="true"></div>

    <header class="title-slide__heading">
      <slot name="title" />
      <slot name="subtitle" />
      <div class="title-slide__rule" aria-hidden="true"></div>
      <footer class="title-slide__footer">
        <slot name="footer" />
      </footer>
    </header>

    <main class="title-slide__visual">
      <slot name="visual" />
    </main>
  </div>
</template>

<style scoped>
.title-slide {
  background:
    radial-gradient(circle at 84% 18%, color-mix(in srgb, var(--accent-soft) 68%, transparent), transparent 30%),
    radial-gradient(circle at 62% 82%, color-mix(in srgb, var(--secondary-soft) 52%, transparent), transparent 35%),
    var(--bg);
  box-sizing: border-box;
  display: grid;
  grid-template-columns: minmax(0, 1.3fr) minmax(15rem, 0.7fr);
  column-gap: 2.6rem;
  height: 100%;
  overflow: hidden;
  padding: 3.1rem 3.8rem;
  position: relative;
}

.title-slide__dot-field {
  background-image: radial-gradient(circle, color-mix(in srgb, var(--secondary) 38%, transparent) 1.15px, transparent 1.3px);
  background-size: 18px 18px;
  height: 31rem;
  mask-image: radial-gradient(ellipse at center, black 18%, transparent 69%);
  opacity: 0.5;
  position: absolute;
  right: -6rem;
  top: -9rem;
  transform: perspective(34rem) rotateX(48deg) rotateZ(-18deg) skewX(-8deg);
  transform-origin: center;
  width: 39rem;
}

.title-slide__heading,
.title-slide__visual {
  position: relative;
  z-index: 1;
}

.title-slide__heading {
  align-self: center;
  display: flex;
  flex-direction: column;
}

.title-slide__heading :deep(h1) {
  color: var(--ink);
  font-size: 4.25rem;
  font-weight: 800;
  letter-spacing: -0.065em;
  line-height: 0.91;
  margin: 0 0 1.5rem;
  max-width: 36rem;
}

.title-slide__heading :deep(h1 em) {
  color: var(--accent);
  font-style: normal;
}

.title-slide__heading :deep(p) {
  color: var(--muted);
  font-size: 1.22rem;
  line-height: 1.42;
  max-width: 30rem;
}

.title-slide__visual {
  align-self: center;
  min-width: 0;
}

.title-slide__visual :deep(.mermaid) {
  display: flex;
  justify-content: center;
  width: 100%;
}

.title-slide__rule {
  background: var(--accent);
  height: 4px;
  margin: 2.2rem 0 1.25rem;
  width: 5.25rem;
}

.title-slide__footer {
  color: var(--muted);
  font-size: 0.94rem;
  font-weight: 500;
  line-height: 1.4;
}

.title-slide__footer :deep(p) {
  margin: 0;
}

.title-slide__footer :deep(strong) {
  color: var(--ink);
  font-size: 1.05rem;
  font-weight: 700;
}
</style>
