---
layout: title
---

::title::

# Deep Learning for *Image Mining*

::subtitle::

From visual descriptors to foundation representations

::visual::

```mermaid {theme: 'neutral', scale: 0.9}
%%{init: {"themeVariables": {"fontSize": "16px"}, "flowchart": {"nodeSpacing": 22, "rankSpacing": 34, "diagramPadding": 4}}}%%
flowchart TB
  handcrafted("Handcrafted<br/>descriptors") --> deep("Deep<br/>representations") --> foundation("Foundation<br/>representations")
  linkStyle default stroke:#df2439,stroke-width:2px
```

::footer::

Arturo Mendoza · IPP–Université Paris-Saclay · 2026

<div class="cover-contact">
  <a class="personal-email" href="mailto:arturo.mendoza.quispe@gmail.com">
    arturo.mendoza.quispe@gmail.com
  </a>
  <span class="separator"> · </span>
  <a class="work-email" href="mailto:arturo.mendoza-quispe@safrangroup.com">
    arturo.mendoza-quispe@safrangroup.com
  </a>
</div>

<style scoped>
.cover-contact {
  margin-top: 0.2rem;
  font-size: 0.75em;
  line-height: 1.3;
  opacity: 0.82;
}
.personal-email {
  color: color-mix(in srgb, var(--accent) 62%, var(--muted));
}
.work-email {
  color: color-mix(in srgb, var(--accent) 62%, var(--muted));
}
.separator {
  color: var(--muted);
}
</style>

<!--
**Purpose:** Set the scope: how learned representations support image mining

**Say:** We will follow how visual representations moved from manually specified features to reusable models that transfer across tasks

**Transition:** Begin with the representation decision shared by every image-mining system
-->
