

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
