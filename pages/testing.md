
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
layout: center
---

<CenteredImage 
  title="VM Tests in Action"
  src="vm-tests.gif"
  alt="VM tests running in action"
/>

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

<RequirementsChecklist :checked="[true, true, true, true]" />