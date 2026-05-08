---
description: "Generate NixOS-focused testing strategies"
---

# Test Strategy

Generate a testing plan for the requested NixOS change using these concrete techniques.

## Config validation
- Run `nix flake check --no-build` to catch type errors and assertion failures.

## Module evaluation
- Evaluate specific options: `nix eval .#nixosConfigurations.<host>.config.<option>` to verify values resolve correctly.
- Use `--show-trace` when evaluation fails to find the source.

## Build dry-run
- Run `nix build --dry-run .#nixosConfigurations.<host>.config.system.build.toplevel` to verify the derivation builds without fetching everything.

## VM integration tests
- Write a `nixosTest` block for service-level integration testing when the change involves systemd services or networking.
- Evaluate the test with `nix build .#checks.x86_64-linux.<testName>`.

## Service smoke tests
- After deploy, verify with `systemctl status <service>` and `journalctl -u <service> --no-pager -n 50`.
- Check ports with `ss -tlnp | grep <port>`.

## Generation diffing
- Compare before/after closures: `nix profile diff-closures --profile /nix/var/nix/profiles/system`.
- List recent generations to confirm the new one activated.

## Home Manager validation
- Run `home-manager generations` to confirm HM config applied.
- Check specific HM-managed files exist at their expected paths.
