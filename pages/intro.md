---
layout: two-cols
---

# About me

- Giannin

<v-clicks depth="2">

- Software Engineer @ Puzzle ITC
- Things that I like:
  - Nix (not an expert though)
  - Dungeons and Dragons
  - Traveling
- Things that I do not like
  - People that make long lists about themselves
  - Ads

</v-clicks>

::right::

<div class="flex justify-center items-center h-full text-9xl transition-all">
  <div v-if="$clicks === 0">👨‍💻</div>
  <div v-if="$clicks === 1">🧩</div>
  <div v-if="$clicks === 2">♥️</div>
  <div v-if="$clicks === 3">❄️</div>
  <div v-if="$clicks === 4">🐉</div>
  <div v-if="$clicks === 5">🚄</div>
  <div v-if="$clicks === 6">💔</div>
  <div v-if="$clicks === 7">🗨️</div>
  <div v-if="$clicks >= 8">🤬</div>
</div>

---



---
layout: quote
---

<BigQuote>
  "I want a network-wide adblocker so that I have to endure less ads..."
  <template #author>
    - Me, October 2025
  </template>
</BigQuote>

---
layout: quote
---

<BigQuote>
  "...using Nix!"
  <template #author>
    - Me, October 2025 <br> (Possibly Delusional)
  </template>
</BigQuote>
