
# Configuration
<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'settings.nix' },
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

# Configuration

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'flake.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|6-8|17}
{
  inputs.nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-25.11";
  outputs =
    { nixpkgs }:
    let
      configModule = {
        _module.args.imageConfig = (import ./settings.nix);
      };
    in
    {
      nixosConfigurations = {
        adguard-pi = lib.nixosSystem {
          system = "aarch64-linux";
          modules = [
            "${nixpkgs}/nixos/modules/installer/sd-card/sd-image-aarch64.nix"
            ./image/configuration.nix
            configModule
          ];
        };
      };
    };
}
```
````

---

# Configuration
<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|3|13-16|10-12}
{
  lib,
  imageConfig
}:
{
  services.adguardhome = {
    enable = true;
    # ...
    settings = {
      querylog.enabled = false;
      dhcp.enabled = false;
      anonymize_client_ip = true;
      dns = {
        upstream_dns = imageConfig.adguard.dns.upstreams;
        bootstrap_dns = imageConfig.adguard.dns.bootstraps;
      };
    };
  };
  networking.firewall = {
    # ...
  };
}
```

````

---

# Validate Config

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|6-12|7-10}
{
  lib,
  imageConfig
}:
{
  assertions = [
    {
      assertion = builtins.length upstreams != 0 upstreams;
      message = "Error: At least one upstream DNS needs to be defined";
    }
    # ...
  ]

  services.adguardhome = {
    # ...
  };
  networking.firewall = {
    # ...
  };
}
```

````

---

# Validate Config

````md magic-move {lines:true}

```shell {all|22-23}
nix build .#nixosConfigurations.adguard-pi.config.system.build.sdImage
warning: Git tree '/home/giannin/Documents/Github/NIX/adguard-nixos-raspberry-image' is dirty
error:
       … while evaluating attribute 'buildCommand' of derivation 'nixos-image-sd-card-25.11.20260320.812b398-...'
         at /nix/store/8ggk1060asqg0dl17gbysd9zknlzhjca-source/nixos/modules/installer/sd-card/sd-image.nix:269:9:
          268|
          269|         buildCommand = ''
             |         ^
          270|           mkdir -p $out/nix-support $out/sd-image

       … while evaluating the option `sdImage.populateRootCommands':

       … while evaluating definitions from `/nix/store/8ggk1060asqg0dl17gbysd9zknlzhjca-source/nixos/modules/...':

       … while evaluating the option `system.build.toplevel':

       … while evaluating definitions from `/nix/store/8ggk1060asqg0dl17gbysd9zknlzhjca-source/nixos/modules/...':

       (stack trace truncated; use '--show-trace' to show the full, detailed trace)

       error:
       Failed assertions:
       - Error: At least one upstream DNS needs to be defined
```

````

---

<RequirementsChecklist :checked="[true, true, true, false]" />