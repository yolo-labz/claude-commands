---
description: "Debug NixOS configuration issues with systematic troubleshooting"
---

# Debug Config

Systematically diagnose the NixOS configuration issue. Follow these steps in order.

1. **Collect errors**: run `nix flake check --no-build 2>&1` and capture the full output.
2. **Check system state**: run `systemctl --failed` and `journalctl -xe -b --no-pager -n 100` for runtime failures.
3. **Evaluate the failing option**: run `nix eval .#nixosConfigurations.<host>.config.<path>` to test specific option values.
4. **Trace evaluation errors**: if you hit infinite recursion or missing attributes, re-run with `nix eval --show-trace` to get the full stack.
5. **Isolate the culprit**: comment out module imports in the host's `configuration.nix` and binary-search which module causes the failure.
6. **Check disk space**: run `df -h /` and `du -sh /nix/store` — build failures are often caused by a full disk.
7. **Rollback if needed**: suggest `nixos-rebuild switch --rollback` or select a previous generation from the GRUB boot menu.

Hosts in this flake: desktop, laptop, server (NixOS), macbook-pro (nix-darwin).

Replace `<host>` with the affected host. Replace `<path>` with the option path the user is investigating.
