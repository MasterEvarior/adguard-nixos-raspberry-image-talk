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