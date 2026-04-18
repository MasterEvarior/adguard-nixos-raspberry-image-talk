---
theme: dracula
highlighter: shiki
lineNumbers: false
fonts:
  sans: "Inter"
  mono: "JetBrains Mono"
transition: fade-in
layout: center
zoom: 1.8
magicMoveDuration: 250
addons:
  - fancy-arrow
  - slidev-addon-excalidraw
---

# Declarative Adblocking 

with Nix and a Raspberry Pis 


---
src: ./pages/intro.md
---

---

<RequirementsChecklist />

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

# Result

<iframe 
  src="http://localhost:8080"
  width="100%" 
  height="400px" 
  frameborder="0">
</iframe> 

---

# Result

<iframe
  src="http://localhost:9100"
  width="100%" 
  height="400px" 
  frameborder="0">
</iframe> 

---
layout: center
---

<CenteredImage 
  title="The Rest of the Owl"
  src="owl.png"
/>


---

# Questions? Ideas? Improvements?

<Links :links="[
    { title: 'Source Code', url: 'https://github.com/MasterEvarior/adguard-nixos-raspberry-image' },
    { title: 'Presentation', url: 'https://github.com/MasterEvarior/adguard-nixos-raspberry-image-talk' }
]" />