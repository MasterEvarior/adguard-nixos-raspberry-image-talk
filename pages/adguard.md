

# AdGuardHome

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all|3-5}
{...}:
{
  services.adguardhome = {
    enable = true;
  };
}
```

```nix {all|5-8}
{...}:
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


```nix {all|9-13}
{...}:
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

```nix {9-13}
{...}:
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

<div v-click>
  <FancyArrow
      from="(295, 315)"
      to="(520, 175)"
      arc="0.2"
      color="red"
  />
</div>

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
{...}:
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
{...}:
{
  services.adguardhome = {
    # ...
  };
}
```

```nix {all|7-10}
{...}:
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
layout: two-cols-header
style:
  column-gap: 2rem
---

# Integrate Module into NixOS

::left::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/adguard.nix' },
  ]" 
/>
```nix
{...}:
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

::right::

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
<div v-click at="1">
  <FancyArrow
      from="(250, 130)"
      to="(540, 210)"
      arc="-0.2"
      color="red"
  />
</div>


---
layout: two-cols-header
style:
  column-gap: 2rem
---

# Integrate Module into NixOS

::left::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/services/default.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all}
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

::right::

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
}
```
````

<div v-click at="1">
  <FancyArrow
      from="(250, 130)"
      to="(560, 260)"
      arc="-0.2"
      color="red"
  />
</div>


---

# Integrate Module into NixOS

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/configuration.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|15|18}
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
}
```
````

---
layout: two-cols-header
style:
  column-gap: 2rem
---

# Integrate Module into NixOS

::left::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'image/configuration.nix' },
  ]" 
/>
````md magic-move {lines:true}

```nix {all}
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
}
```
````

::right::

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'flake.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|12}
{
  inputs = { nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-25.11"; };

  outputs =
    { nixpkgs }:
    {
      nixosConfigurations = {
        adguard-pi = lib.nixosSystem {
          system = "aarch64-linux";
          modules = [
            "${nixpkgs}/nixos/modules/installer/..."
            ./image/configuration.nix
          ];
        };
      };
    };
}
```
````

<div v-click at="1">
  <FancyArrow
      from="(250, 130)"
      to="(560, 350)"
      arc="-0.2"
      color="red"
  />
</div>




---

# Integrate Module into NixOS

<FileHeaders 
  :clicks="$clicks" 
  :steps="[
    { click: 0, name: 'flake.nix' },
  ]" 
/>
````md magic-move {lines:true}
```nix {all|7|8|9|11|all}
{
  inputs = { nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-25.11"; };

  outputs =
    { nixpkgs }:
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
class: flex flex-col
---

# Build SD-Card Image

<div class="flex-1">
  <Terminal
    persist
    session="build"
  />
</div>

---

<RequirementsChecklist :checked="[true, true, false, false]" />