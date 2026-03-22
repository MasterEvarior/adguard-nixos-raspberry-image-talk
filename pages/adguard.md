

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|6-8}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    enable = true;
  };
}
```

```nix {all|8-11}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    enable = true;
    mutableSettings = true;
    openFirewall = true;
    host = "0.0.0.0";
    port = 80;
  };
}
```


```nix {all|12-16}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    enable = true;
    mutableSettings = true;
    openFirewall = true;
    host = "0.0.0.0";
    port = 80;
    settings = {
      querylog.enabled = false;
      dhcp.enabled = false;
      anonymize_client_ip = true;
    };
  };
}
```

````

---
layout: two-cols-header
style:
  column-gap: 2rem
---

# AdGuardHome

::left::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>

```nix {12-16}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    enable = true;
    mutableSettings = true;
    openFirewall = true;
    host = "0.0.0.0";
    port = 80;
    settings = {
      querylog.enabled = false;
      dhcp.enabled = false;
      anonymize_client_ip = true;
    };
  };
}
```

::right::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'var/lib/AdGuardHome/AdGuardHome.yaml' },
  ]" 
/>

```yaml {all}
querylog:
  enabled: false
dhcp:
  enabled: false
anonymize_client_ip: true
```

---

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    enable = true;
    mutableSettings = true;
    openFirewall = true;
    host = "0.0.0.0";
    port = 80;
    settings = {
      querylog.enabled = false;
      dhcp.enabled = false;
      anonymize_client_ip = true;
    };
  };
}
```

```nix {all}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    # ...
  };
}
```

```nix {all|10-13}
{
  lib,
  ...
}:
{
  services.adguardhome = {
    # ...
  };

  networking.firewall = {
    allowedUDPPorts = [ 53 ];
    allowedTCPPorts = [ 53 ];
  };
}
```
````

---

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/default.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|4}
{ ... }:
{
  imports = [
    ./adguard.nix
    ./node-exporter.nix
    ./ssh.nix
  ];
}
```

````


---

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/configuration.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|7}
{
  pkgs,
  ...
}:
{
  imports = [
    ./services
    ./settings
  ];
  nix.settings.experimental-features = [
    "nix-command"
    "flakes"
  ];
  environment.systemPackages = with pkgs; [
    micro
  ];
  networking = {
    hostName = "adguard";
    networkmanager.enable = true;
  };
  system.stateVersion = "25.05";
}
```
````

---

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'flake.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|15|14}
{
  inputs = { nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-25.11"; };

  outputs =
    { nixpkgs }:
    let
      lib = nixpkgs.lib;
    in
    {
      nixosConfigurations = {
        adguard-pi = lib.nixosSystem {
          system = "aarch64-linux";
          modules = [
            "${nixpkgs}/nixos/modules/installer/sd-card/sd-image-aarch64.nix"
            ./image/configuration.nix
          ];
        };
      };
    };
}
```
````

---
layout: center
---

<CenteredImage 
  title="AdGuardHome"
  src="build.gif"
  alt=""
/>