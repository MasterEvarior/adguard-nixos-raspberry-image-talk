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
addons:
  - fancy-arrow
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
layout: center
---

<CenteredImage 
  title="The Rest of the Owl"
  src="owl.png"
/>



---
layout: center
---

<CenteredImage 
  title="Result"
  src="result.png"
/>

---

# Links

<Links :links="[
    { title: 'Source Code', url: 'https://github.com/MasterEvarior/adguard-nixos-raspberry-image' },
    { title: 'Presentation', url: 'https://github.com/MasterEvarior/adguard-nixos-raspberry-image-talk' }
]" />

---
layout: center
---

# Questions? Ideas? Improvements?