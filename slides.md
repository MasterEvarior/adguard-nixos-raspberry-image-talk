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
  - People that talk too much about themselves
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

--- 

# Configuration
<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'configuration.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|2-4}
{
  machine = {
    hostname = "adguard";
  };
}
```


```nix {3-8}
{
  machine = { ... };
  user = {
    username = "stan";
    noPassword = true;
    initialPassword = "test123";
    sshKey = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHgX2NzC50UtAF/0/AGWSY2x3EvunwPQ5kHiVkQzyMVN noname";
  };
}
```

```nix {4-18}
{
  machine = { ... };
  user = { ... };
  adguard = {
    dns = {
      upstreams = [
        "https://dns.mullvad.net/dns-query"
        "https://unfiltered.adguard-dns.com/dns-query"
        "https://dns.switch.ch/dns-query"
      ];
      bootstraps = [
        "1.1.1.1"
        "8.8.8.8"
      ];
      upstreamMode = "load_balance";
    };
  ...
  };
}
```

````


---

# Testing
<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'tests/node-exporter/default.nix' },
    { click: 7, name: 'tests/node-exporter/script.py' }
  ]" 
/>
````md magic-move {lines:true}

```nix [tests/node-exporter] {all|2}
pkgs.testers.runNixOSTest {
  name = "node-exporter";
}
```

```nix [tests/node-exporter] {3-4,11|5-10}
pkgs.testers.runNixOSTest {
  name = "node-exporter";
  nodes = {
    server = # First Machine
      { pkgs, lib, ... }:
      {
        imports = [
          ../../image/configuration.nix
        ];
      };
  };
}
```

```nix [tests/node-exporter] {12}
pkgs.testers.runNixOSTest {
  name = "node-exporter";
  nodes = {
    server = # First Machine
      { pkgs, lib, ... }:
      {
        imports = [
          ../../image/configuration.nix
        ];
        networking.hostName = lib.mkForce "server";
        virtualisation.graphics = false;
        environment.systemPackages = [ pkgs.curlMinimal ];
      };
  };
}
```

```nix [tests/node-exporter] {14-19}
pkgs.testers.runNixOSTest {
  name = "node-exporter";
  nodes = {
    server = # First Machine
      { pkgs, lib, ... }:
      {
        imports = [
          ../../image/configuration.nix
        ];
        networking.hostName = lib.mkForce "server";
        virtualisation.graphics = false;
        environment.systemPackages = [ pkgs.curlMinimal ];
      };
    client = # Second Machine
      { pkgs, ... }:
      {
        environment.systemPackages = [ pkgs.curlMinimal ];
        virtualisation.graphics = false;
      };
  };
}
```

```nix [tests/node-exporter/default.nix] {21}
pkgs.testers.runNixOSTest {
  name = "node-exporter";
  nodes = {
    server = # First Machine
      { pkgs, lib, ... }:
      {
        imports = [
          ../../image/configuration.nix
        ];
        networking.hostName = lib.mkForce "server";
        virtualisation.graphics = false;
        environment.systemPackages = [ pkgs.curlMinimal ];
      };
    client = # Second Machine
      { pkgs, ... }:
      {
        environment.systemPackages = [ pkgs.curlMinimal ];
        virtualisation.graphics = false;
      };
  };
  testScript = builtins.readFile ./script.py;
}
```

```python [tests/node-exporter/script.py] {1|3-4}
start_all()

server.wait_for_unit("prometheus-node-exporter.service")
server.wait_for_open_port(9100)
```

```python [tests/node-exporter/script.py] {6-7|9-10}
start_all()

server.wait_for_unit("prometheus-node-exporter.service")
server.wait_for_open_port(9100)

with subtest("Node Exporter is reachable from server"):
    server.succeed("curl --fail http://127.0.0.1:9100/metrics")

with subtest("Node Exporter is reachable from external client"):
    client.succeed("curl --fail http://server:9100/metrics")
```

````

---

# CI
<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: '.github/workflows/ci.yaml' },
  ]" 
/>
````md magic-move {lines:true}
```yaml {all|12-13|14-18|19-20}
name: CI
on:
  ...
jobs:
  test:
    name: "Execute VM tests"
    runs-on: ubuntu-latest
    needs: lint
    permissions:
      contents: read
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
      - name: Install Nix
        uses: cachix/install-nix-action@v31.10.1
        with:
          enable_kvm: true
          extra_nix_config: "system-features = nixos-test benchmark big-parallel kvm"
      - name: Run All Tests and Checks
        run: nix develop --command just test-all
```
````
---

# Learnings

---

# Still To Do

- Configuration
- ...

---

# Links