

# Testing

- Is the admin UI reachable under `localhost:80`?
- Is the admin UI reachable from an external client?

<Excalidraw
  drawFilePath="./tests-simple.excalidraw"
  class="w-[900px]"
  :darkMode="true"
  :background="false"
/>


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

```nix [tests/adguard/default.nix] {all|2}
pkgs.testers.runNixOSTest {
  name = "adguard";
}
```

```nix [tests/adguard/default.nix] {3-4,11|5-10}
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

```nix [tests/adguard/default.nix] {12}
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

```nix [tests/adguard/default.nix] {14-19}
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

```nix [tests/adguard/default.nix] {21}
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

```python [tests/adguard/script.py] {all|1|3-5|7-15}
start_all()

server.wait_for_unit("adguardhome.service")
server.wait_for_open_port(53)
server.wait_for_open_port(80)

with subtest("AdGuard Home is reachable from server"):
    server.succeed(
        "curl --fail http://127.0.0.1:80 | grep -q '<title>AdGuard Home</title>'"
    )

with subtest("AdGuard Home is reachable from client"):
    client.succeed(
        "curl --fail http://server:80 | grep -q '<title>AdGuard Home</title>'"
    )
```

```python [tests/adguard/script.py] {all|1|7-20|8-11|13|15-20}
import json

start_all()

# ...

with subtest("AdGuard Home has loaded specified lists via API"):
    response = server.succeed(
        "curl -s http://127.0.0.1:80/control/filtering/status"
    ).strip()
    data = json.loads(response)

    filter_names = [f["name"] for f in data.get("filters", [])]

    expected_filters = ["URL House", "AdAway Default Blocklist"]
    for name in expected_filters:
        if name in filter_names:
            server.log(f"Verified filter: {name}")
        else:
            server.fail(f"Filter '{name}' not found in AdGuard. Found: {filter_names}")

```

````

---

# Testing

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'flake.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|16-18}
{
  inputs = { nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-25.11"; };

  outputs =
    { nixpkgs }:
    let
      configModule = {
        _module.args.imageConfig = (import ./settings.nix);
      };
    in
    {
      nixosConfigurations = {
        # ...
      };

      checks.x86_64-linux.adguard = import ./tests/adguard {
        inherit pkgs;
      };
    };
}
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

# VM Tests in Action

<v-clicks>

- Starts a VM with our configured NixOS as the server  
  ```log
  server: (finished: waiting for the VM to finish booting, in 61.07 seconds)
  server: (finished: waiting for unit adguardhome.service, in 68.88 seconds)
  server: waiting for TCP port 53 on localhost
  server: (finished: waiting for TCP port 53 on localhost, in 3.88 seconds)
  ```
- Starts the client with our configured packages
- Runs the python script and our tests  
  ```log
  client: (finished: must succeed: curl --fail http://server:80 | grep -q '<title>AdGuard Home</title>', in 0.62 seconds)
  (finished: subtest: AdGuard Home is reachable from client, in 0.62 seconds)
  ```
- Fail if any test is not successful
- Otherwise, simply quits with exit code 0  
  ```log
  test script finished in 76.62s
  ```

</v-clicks>

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

<CenteredImage 
  title="CI"
  src="gh-actions.png"
  alt="Successful test run on GitHub Actions"
/>

---

<RequirementsChecklist :checked="[true, true, true, true]" />