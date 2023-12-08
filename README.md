# Shopify CLI packaged for Nix

This is the [shopify-cli](https://github.com/Shopify/cli) packaged for Nix.

## Installation

```sh
nix profile install github:digital-brew/nix-shopify-cli
```

Alternatively you can just run the CLI directly without permanently installing it:

```sh
nix run github:digital-brew/nix-shopify-cli -- <args>
```

# Usage as a flake

Add nix-shopify-cli to your `flake.nix`:

```nix
{
  inputs.nix-shopify-cli.url = "github:digital-brew/nix-shopify-cli";

  outputs = { self, nix-shopify-cli }: {
    # Use in your outputs
    environment.systemPackages = [
      nix-shopify-cli.packages.${system}.default
    ];
  };
}

```

## Supported Systems

- `x86_64-linux`
- `x86_64-darwin`
- `aarch64-linux`
- `aarch64-darwin`

## Build

```sh
nix build
nix flake check
```

## How to build a new version of `shopify`

1. Update the `package.json` with the new version
2. Run `npm install && rm -rf node_modules` (we only care about the lock file).
3. Run `nix flake check`, Nix will complain that the download hash is outdated.
4. Update `downloadHash` in `default.nix` with the new hash.
5. Run `nix flake check` again, now it should build the new version.
6. Run `nix run -- <some args>` to test the new version.
7. When you are happy with the new version:
   1. push the changes to GitHub
   2. rebuild your nix config
