# schemathesis.nix ❄️

Nix packaging for [Schemathesis](https://github.com/schemathesis/schemathesis) (a property-based API testing tool for OpenAPI/GraphQL schemas) along with Allure reporting integration.

It depends only on `nixpkgs`, ensuring that consumer repositories do not pull along unnecessary NixOS/host system dependencies.

## Usage

### 1. Running Locally (from cloned repository)

- Check version:

  ```bash
  nix run . -- --version
  ```

- Run tests against an API schema:

  ```bash
  nix run . -- run https://example.com/openapi.json
  ```

- Enter an interactive shell with `schemathesis` in `PATH`:

  ```bash
  nix shell .
  ```

### 2. Running Remotely (from any directory or machine)

You can run or enter a shell with `schemathesis` directly from GitHub without cloning:

- Run directly:

  ```bash
  nix run "github:enola-dev/schemathesis.nix" -- --version
  ```

- Interactive shell:

  ```bash
  nix shell "github:enola-dev/schemathesis.nix"
  ```

### 3. Consuming in another `flake.nix`

Add this flake to your project's inputs:

```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";

    schemathesis = {
      url = "github:enola-dev/schemathesis.nix";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, schemathesis, ... }:
    let
      system = "x86_64-linux";
      pkgs = nixpkgs.legacyPackages.${system};
    in {
      devShells.${system}.default = pkgs.mkShellNoCC {
        packages = [
          schemathesis.packages.${system}.default
        ];
      };
    };
}
```
