
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

# Configuration
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

<RequirementsChecklist :checked="[true, true, true, false]" />