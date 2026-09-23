<template>
  <div class="wrap">
    <svg viewBox="-8 -8 496 346" role="img" aria-label="Data, architecture, and objective shape the learned representation">
      <path class="tri" d="M240 44 L74 265 L406 265 Z" />
      <text class="hubtext" x="240" y="184">LEARNED</text>
      <text class="hubtext" x="240" y="205">REPRESENTATION</text>
      <g><rect class="corner accent" x="180" y="14" width="120" height="60" rx="14" /><text x="240" y="49">DATA</text></g>
      <g><rect class="corner accent" x="0" y="233" width="140" height="64" rx="14" /><text x="70" y="270">ARCHITECTURE</text></g>
      <g><rect class="corner accent" x="345" y="233" width="135" height="64" rx="14" /><text x="412.5" y="270">OBJECTIVE</text></g>
    </svg>
  </div>
</template>

<style scoped>
.wrap { margin: auto; width: 80%; }
svg { display: block; width: 100%; }
.tri { fill: none; stroke: var(--line); stroke-width: 3; }
.hub { fill: var(--surface); stroke: var(--ink); stroke-width: 2; }
.corner { stroke-width: 2; }
.corner.accent { fill: var(--accent-soft); stroke: var(--accent); }
.corner.secondary { fill: var(--secondary-soft); stroke: var(--secondary); }
text { fill: var(--ink); font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 700; text-anchor: middle; }
.hubtext { font-size: 14px; }
</style>
