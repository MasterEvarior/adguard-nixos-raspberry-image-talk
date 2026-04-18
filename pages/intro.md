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
  <img v-if="$clicks >= 8" src="/ad_1.png" class="absolute bottom-3/8 right-7 w-40 -rotate-6" />
  <img v-if="$clicks >= 8" src="/ad_2.png" class="absolute bottom-1 right-45 w-80 rotate-5" />
  <img v-if="$clicks >= 8" src="/ad_3.png" class="absolute -top-3 right-55 w-100 -rotate-5" />
  <img v-if="$clicks >= 8" src="/ad_4.png" class="absolute -bottom-15 left-20 w-75 -rotate-5" />
</div>

---
layout: quote
---

<BigQuote>
  "I want a network-wide adblocker to avoid ads..."
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


---

# Network-Wide Adblocker?

<div class="flex justify-center">
  <Excalidraw
    drawFilePath="./adguard.excalidraw"
    class="w-[650px]"
    :darkMode="true"
  />
</div>

---
clicks: 1
---

# Network-Wide Adblocker

<img v-if="$clicks === 0" src="/pi-hole.png">
<img v-if="$clicks === 1" src="/adguardhome.png">

