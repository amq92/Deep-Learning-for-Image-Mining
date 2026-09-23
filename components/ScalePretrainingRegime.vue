<template>
  <figure class="scale-regime" aria-label="Comparison of scale across data, model capacity, and training compute">
    <table>
      <thead>
        <tr>
          <th>Axis</th>
          <th>Classical regime</th>
          <th>Large-scale regime</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Data</td>
          <td>ImageNet-1k<br><span>~1.3M images</span></td>
          <td>LAION-5B / web-scale<br><span>~5B pairs</span></td>
        </tr>
        <tr>
          <td>Model</td>
          <td>ResNet-50<br><span>~25M params</span></td>
          <td>ViT-L / ViT-H<br><span>~300M–600M+</span></td>
        </tr>
        <tr>
          <td>Compute</td>
          <td>ImageNet training<br><span>~10^18 FLOPs</span></td>
          <td>Large pretraining<br><span>~10^21–10^23 FLOPs</span></td>
        </tr>
      </tbody>
    </table>
  </figure>
</template>

<style scoped>
.scale-regime {
  margin: 0;
  width: 100%;
}

table {
  border-collapse: collapse;
  border: 1px solid var(--line);
  border-radius: 12px;
  margin: 0 auto;
  overflow: hidden;
  width: 100%;
}

th,
td {
  border: 1px solid var(--line);
  padding: 0.45rem 0.5rem;
  text-align: center;
  vertical-align: middle;
}

th {
  background: var(--surface);
  color: var(--muted);
  font-size: 0.52rem;
  font-weight: 800;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

td {
  color: var(--text);
  font-size: 0.58rem;
  line-height: 1.35;
}

td:first-child {
  color: var(--accent);
  font-weight: 800;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  width: 24%;
}

span {
  color: var(--muted);
  font-size: 0.5rem;
}
</style>
