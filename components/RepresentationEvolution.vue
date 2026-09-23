<template>
  <figure class="evolution" aria-label="A rubber duck image progresses from a fixed descriptor to a task-trained representation and then to a reusable representation serving several tasks">
    <img src="/images/representation/duck-alternate.jpg" alt="Yellow rubber duck" />
    <div class="arrow">↓</div>
    <div class="stage">
      <strong>Fixed descriptor</strong>
      <span>designed by hand</span>
    </div>
    <div class="arrow">↓</div>
    <div class="stage">
      <strong>Task-trained representation</strong>
      <span>learned for one objective</span>
    </div>
    <div class="arrow">↓</div>
    <div class="stage reusable">
      <strong>Reusable representation</strong>
      <span>adapted across tasks</span>
    </div>
    <div class="tasks">
      <span>retrieve</span>
      <span>classify</span>
      <span>segment</span>
    </div>
  </figure>
</template>

<style scoped>
.evolution {
  align-items: center;
  display: flex;
  flex-direction: column;
  margin: 0;
  width: 100%;
}
.evolution img {
  border: 1px solid var(--line);
  border-radius: 12px;
  height: 76px;
  object-fit: cover;
  width: 108px;
}
.arrow {
  color: var(--accent);
  font-size: 1.25rem;
  font-weight: 700;
  height: 1.45rem;
  line-height: 1.45rem;
}
.stage {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 12px;
  padding: 0.55rem 0.75rem;
  text-align: center;
  width: 100%;
}
.stage strong,
.stage span {
  display: block;
}
.stage strong {
  font-size: 0.82rem;
}
.stage span {
  color: var(--muted);
  font-size: 0.68rem;
  margin-top: 0.12rem;
}
.reusable {
  border-color: var(--accent);
}
.tasks {
  display: grid;
  gap: 0.3rem;
  grid-template-columns: repeat(3, 1fr);
  margin-top: 0.45rem;
  width: 100%;
}
.tasks span {
  background: var(--surface-2);
  border-radius: 8px;
  color: var(--muted);
  font-size: 0.58rem;
  font-weight: 700;
  padding: 0.3rem 0.15rem;
  text-align: center;
}
</style>
