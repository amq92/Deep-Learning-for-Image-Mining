<template>
  <figure class="representation-transform">
    <div class="source-column">
      <span class="micro-label">input image</span>
      <div class="source-image">
        <img src="/images/representation/duck-alternate.jpg" alt="A yellow rubber duck" />
      </div>
    </div>

    <div class="branch" aria-hidden="true">
      <span></span><span></span><span></span>
    </div>

    <div class="representations">
      <section class="representation-card concepts">
        <header>
          <span class="card-number">01</span>
          <strong>Concept vector</strong>
        </header>
        <div class="concept-values" aria-label="yellow, rubber, toy, beak">
          <span>yellow</span><span>rubber</span><span>toy</span><span>beak</span>
        </div>
        <p>keeps interpretable properties</p>
      </section>

      <section class="representation-card spatial">
        <header>
          <span class="card-number">02</span>
          <strong>Local region map</strong>
        </header>
        <div class="spatial-content">
          <svg viewBox="0 0 72 72" role="img" aria-label="Irregular local regions preserving color, texture, and edge cues">
            <rect width="72" height="72" rx="7" class="region-background" />
            <g class="regions">
              <path d="M2 2h22l-3 17-19 4z" class="region neutral-1" />
              <path d="M24 2h25l-4 14-24 3z" class="region neutral-2" />
              <path d="M49 2h21v20l-25-6z" class="region neutral-3" />
              <path d="M2 23l19-4 8 13-13 13-14-4z" class="region neutral-3" />
              <path d="M21 19l24-3 9 13-12 10-13-7z" class="region duck-yellow" />
              <path d="M45 16l25 6v17l-16-10z" class="region neutral-1" />
              <path d="M16 45l13-13 13 7-4 15-20 7z" class="region duck-gold" />
              <path d="M42 39l12-10 16 10v17l-18 3-14-5z" class="region duck-yellow" />
              <path d="M2 41l14 4 2 16-16 9z" class="region neutral-2" />
              <path d="M18 61l20-7 14 5-4 11H2z" class="region duck-orange" />
              <path d="M52 59l18-3v14H48z" class="region neutral-3" />
            </g>
            <path d="M29 32l13 7 12-10" class="edge-cue" />
            <circle cx="46" cy="27" r="2.2" class="dark-cue" />
          </svg>
          <div class="local-cues">
            <span>color</span><span>texture</span><span>edges</span>
          </div>
        </div>
        <p>keeps local evidence and boundaries</p>
      </section>

      <section class="representation-card colors">
        <header>
          <span class="card-number">03</span>
          <strong>Color summary</strong>
        </header>
        <div class="color-summary" aria-label="A palette summarizing the image colors">
          <span class="swatch yellow"></span>
          <span class="swatch orange"></span>
          <span class="swatch cream"></span>
          <span class="swatch charcoal"></span>
        </div>
        <p>keeps appearance; discards location</p>
      </section>
    </div>

    <figcaption>Photo: terri_bateman · CC BY 2.0 · Wikimedia Commons</figcaption>
  </figure>
</template>

<style scoped>
.representation-transform {
  align-items: stretch;
  display: grid;
  gap: 0.55rem;
  grid-template-columns: minmax(0, 0.78fr) 1.25rem minmax(0, 1.35fr);
  margin: 0;
  position: relative;
  width: 100%;
}

.source-column {
  align-self: center;
  min-width: 0;
}

.micro-label {
  color: var(--muted);
  display: block;
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.09em;
  margin-bottom: 0.28rem;
  text-align: center;
  text-transform: uppercase;
}

.source-image {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 14px;
  box-shadow: 0 8px 20px var(--shadow);
  overflow: hidden;
  padding: 0.3rem;
}

.source-image img {
  border-radius: 10px;
  display: block;
  aspect-ratio: 1;
  height: auto;
  object-fit: cover;
  object-position: center 32%;
  width: 100%;
}

.branch {
  display: grid;
  grid-template-rows: repeat(3, 1fr);
}

.branch span {
  align-self: center;
  border-top: 2px solid var(--accent);
  display: block;
  position: relative;
}

.branch span::after {
  border-bottom: 4px solid transparent;
  border-left: 6px solid var(--accent);
  border-top: 4px solid transparent;
  content: '';
  position: absolute;
  right: -1px;
  top: -5px;
}

.representations {
  display: grid;
  gap: 0.48rem;
  grid-template-rows: repeat(3, minmax(0, 1fr));
}

.representation-card {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 11px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-width: 0;
  padding: 0.48rem 0.58rem;
}

.representation-card header {
  align-items: baseline;
  display: flex;
  gap: 0.36rem;
}

.representation-card strong {
  font-family: 'Manrope', sans-serif;
  font-size: 0.82rem;
}

.card-number {
  color: var(--accent);
  font-size: 0.62rem;
  font-weight: 800;
}

.representation-card p {
  color: var(--muted);
  font-size: 0.7rem;
  line-height: 1.2;
  margin: 0.28rem 0 0;
}

.concept-values,
.embedding-values {
  display: flex;
  flex-wrap: wrap;
  gap: 0.25rem;
  margin-top: 0.34rem;
}

.concept-values span {
  background: var(--secondary-soft);
  border-radius: 999px;
  color: var(--secondary);
  font-size: 0.62rem;
  font-weight: 700;
  padding: 0.18rem 0.33rem;
}

.spatial-content {
  align-items: center;
  display: grid;
  gap: 0.45rem;
  grid-template-columns: 4.6rem 1fr;
  margin-top: 0.25rem;
}

.spatial-content svg {
  display: block;
  width: 100%;
}

.region-background { fill: var(--surface-2); }
.region {
  stroke: color-mix(in srgb, var(--surface) 88%, transparent);
  stroke-linejoin: round;
  stroke-width: 1.2;
}
.neutral-1 { fill: #d8d2c6; }
.neutral-2 { fill: #b9b4aa; }
.neutral-3 { fill: #88857f; }
.duck-yellow { fill: #f3c51c; }
.duck-gold { fill: #dfa817; }
.duck-orange { fill: #e87822; }
.edge-cue {
  fill: none;
  stroke: var(--secondary);
  stroke-linecap: round;
  stroke-width: 2;
}
.dark-cue { fill: #373431; }

.local-cues {
  display: flex;
  flex-wrap: wrap;
  gap: 0.2rem;
}

.local-cues span {
  background: var(--secondary-soft);
  border-radius: 999px;
  color: var(--secondary);
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.14rem 0.28rem;
}

.color-summary {
  display: flex;
  height: 1.55rem;
  margin-top: 0.35rem;
  overflow: hidden;
  width: 100%;
}

.swatch { flex: 1; }
.swatch:first-child { border-radius: 6px 0 0 6px; }
.swatch:last-child { border-radius: 0 6px 6px 0; }
.yellow { background: #f3c51c; }
.orange { background: #e87822; }
.cream { background: #e9dfc8; }
.charcoal { background: #373431; }

figcaption {
  bottom: -0.78rem;
  color: var(--muted);
  font-size: 0.42rem;
  grid-column: 1 / -1;
  line-height: 1;
  position: absolute;
  right: 0;
}
</style>
