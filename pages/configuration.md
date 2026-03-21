
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
