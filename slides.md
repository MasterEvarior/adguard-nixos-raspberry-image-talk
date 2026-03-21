---
theme: dracula
highlighter: shiki
lineNumbers: false
fonts:
  sans: 'Inter'
  mono: 'JetBrains Mono'
transition: fade-in
layout: center
zoom: 1.8
magicMoveDuration: 250
---

# Declarative Adblocking 

with Nix and a Raspberry Pis 


---
src: ./pages/intro.md
---

---
layout: two-cols
---
# Requirements


- Needs to run on hardware I already have (Raspberry Pi 3b+)

<v-clicks>

- Testable (at least somewhat)
- Export metrics in some way, shape or form
- Configurable during build time
- Require no configuration after installation

</v-clicks>

::right::

<div class="flex justify-center items-center h-full text-9xl transition-all">
  <div v-if="$clicks === 0">🍓</div>
  <div v-if="$clicks === 1">🧪</div>
  <div v-if="$clicks === 2">📈</div>
  <div v-if="$clicks === 3">⚙️</div>
  <div v-if="$clicks === 4">🛏️</div>
</div>

---
src: ./pages/adguard.md
---

---
src: ./pages/configuration.md 
---


---
src: ./pages/testing.md
---


---
src: ./pages/ci.md
---

---

# Results

---

# Links