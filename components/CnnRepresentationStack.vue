<template>
  <figure class="stack" aria-label="A CNN transforms pixels through a stack of spatial feature tensors into task evidence">
    <div class="stage input-stage">
      <span class="stage-label">Input</span>
      <img src="/images/representation/duck-alternate.jpg" alt="Yellow rubber duck" />
      <strong>Pixels</strong>
      <small>224 × 224 × 3</small>
    </div>

    <div class="transform">
      <span>learned stage</span><b aria-hidden="true">→</b>
    </div>

    <div class="stage">
      <span class="stage-label">Early</span>
      <div class="tensor tensor-early" aria-hidden="true">
        <span class="slice edge-a"></span><span class="slice edge-b"></span><span class="slice edge-c"></span>
      </div>
      <strong>Local responses</strong>
      <small>112 × 112 × 64</small>
    </div>

    <div class="transform">
      <span>learned stage</span><b aria-hidden="true">→</b>
    </div>

    <div class="stage">
      <span class="stage-label">Middle</span>
      <div class="tensor tensor-middle" aria-hidden="true">
        <span class="slice region-a"></span><span class="slice region-b"></span><span class="slice region-c"></span>
      </div>
      <strong>Combined evidence</strong>
      <small>28 × 28 × 256</small>
    </div>

    <div class="transform">
      <span>learned stage</span><b aria-hidden="true">→</b>
    </div>

    <div class="stage">
      <span class="stage-label">Deep</span>
      <div class="tensor tensor-deep" aria-hidden="true">
        <span class="slice object-a"></span><span class="slice object-b"></span><span class="slice object-c"></span>
      </div>
      <strong>Task evidence</strong>
      <small>7 × 7 × 2048</small>
    </div>

    <div class="trend spatial"><span>spatial detail</span><b>decreases</b></div>
    <div class="trend channels"><span>channels &amp; context</span><b>increase</b></div>

    <figcaption>Illustrative tensor shapes · each stack shows only three channels</figcaption>
  </figure>
</template>

<style scoped>
.stack {
  align-items: center;
  display: grid;
  gap: 0.35rem;
  grid-template-columns: 1fr 0.62fr 1fr 0.62fr 1fr 0.62fr 1fr;
  grid-template-rows: auto auto auto;
  margin: 0;
  padding: 0.6rem 0 1.45rem;
  position: relative;
  width: 100%;
}
.stage { align-items: center; display: flex; flex-direction: column; min-width: 0; text-align: center; }
.stage-label { color: var(--accent); font-size: 0.67rem; font-weight: 800; letter-spacing: 0.07em; margin-bottom: 0.45rem; text-transform: uppercase; }
.stage strong { font-size: 0.76rem; line-height: 1.15; margin-top: 0.55rem; }
.stage small { color: var(--muted); font-family: 'Fira Code', monospace; font-size: 0.57rem; line-height: 1.15; margin-top: 0.2rem; }
.input-stage img { border: 1px solid var(--line); border-radius: 12px; box-shadow: 0 7px 18px var(--shadow); height: 5.2rem; object-fit: cover; object-position: center 32%; width: 5.2rem; }
.transform { align-items: center; display: flex; flex-direction: column; transform: translateY(-0.1rem); }
.transform span { color: var(--muted); font-size: 0.49rem; font-weight: 700; white-space: nowrap; }
.transform b { color: var(--accent); font-size: 1.4rem; line-height: 1; }
.tensor { height: 5.2rem; position: relative; width: 5.1rem; }
.slice { background-color: var(--surface); border: 1px solid var(--line); border-radius: 9px; box-shadow: 0 5px 12px var(--shadow); height: 4.45rem; left: 0; position: absolute; top: 0; width: 4.45rem; }
.slice:nth-child(2) { left: 0.28rem; top: 0.28rem; }
.slice:nth-child(3) { left: 0.56rem; top: 0.56rem; }
.tensor-middle { transform: scale(0.9); }
.tensor-deep { transform: scale(0.8); }
.edge-a { background: repeating-linear-gradient(45deg, var(--surface-2) 0 0.5rem, var(--line) 0.5rem 0.64rem); }
.edge-b { background: linear-gradient(90deg, var(--surface-2) 44%, var(--secondary) 45% 54%, var(--surface-2) 55%); }
.edge-c { background: radial-gradient(circle at 63% 38%, var(--accent) 0 0.36rem, transparent 0.4rem), var(--surface); }
.region-a { background: linear-gradient(145deg, var(--secondary-soft) 0 42%, var(--surface-2) 43% 67%, var(--accent-soft) 68%); }
.region-b { background: radial-gradient(ellipse at 55% 53%, var(--secondary) 0 24%, transparent 25%), var(--surface-2); }
.region-c { background: linear-gradient(25deg, var(--surface-2) 0 34%, var(--accent-soft) 35% 62%, var(--surface) 63%); }
.object-a { background: radial-gradient(ellipse at 51% 58%, var(--accent-soft) 0 35%, transparent 36%), var(--surface-2); }
.object-b { background: radial-gradient(circle at 67% 35%, var(--secondary) 0 9%, transparent 10%), var(--surface); }
.object-c { background: radial-gradient(ellipse at 54% 55%, var(--accent) 0 25%, var(--accent-soft) 26% 42%, transparent 43%), var(--surface-2); }
.trend { align-items: center; border-radius: 999px; display: flex; font-size: 0.61rem; grid-column: 1 / -1; justify-content: space-between; padding: 0.25rem 0.7rem; }
.trend span { font-weight: 700; }
.trend b { color: var(--accent); }
.spatial { background: var(--surface-2); grid-row: 2; }
.channels { background: var(--accent-soft); grid-row: 3; }
figcaption { bottom: 0; color: var(--muted); font-size: 0.57rem; grid-column: 1 / -1; left: 0; position: absolute; text-align: center; width: 100%; }
</style>
