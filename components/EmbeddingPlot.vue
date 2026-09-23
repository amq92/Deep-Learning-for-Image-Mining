<template>
  <div class="plot">
    <div class="axes"><span>representation space</span></div>
    <div v-for="n in 4" :key="`a${n}`" class="point a" :style="{ left: `${17+n*5}%`, top: `${34+(n%2)*8}%` }">●</div>
    <div v-for="n in 4" :key="`b${n}`" class="point b" :style="{ left: `${62+n*4}%`, top: `${56-(n%2)*9}%` }">◆</div>
    <div class="near">same identity<br><b>close</b></div>
    <div class="far">different meaning<br><b>separate</b></div>
  </div>
</template>

<style scoped>
.plot {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 16px;
  box-sizing: border-box;
  height: 245px;
  min-width: 0;
  overflow: hidden;
  position: relative;
  width: 100%;
}
.axes { border-bottom: 1px solid var(--line); border-left: 1px solid var(--line); bottom: 30px; left: 36px; position: absolute; right: 25px; top: 25px; }
.axes span { bottom: -24px; color: var(--muted); font-size: 10px; position: absolute; right: 0; }
.point { font-size: 24px; position: absolute; transform: translate(-50%, -50%); }
.point.a { color: var(--secondary); }
.point.b { color: var(--accent); }
.near, .far { color: var(--muted); font-size: 11px; line-height: 1.2; position: absolute; text-align: center; }
.near { left: 16%; top: 65%; }
.far { right: 8%; top: 22%; }
b { color: var(--ink); }
</style>
