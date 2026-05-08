---
description: "Smart NixOS system rebuild with host detection and validation"
---

# Nix Rebuild

1. Run `git status` in the flake directory. Warn if there are uncommitted changes — unstaged files won't be included in the build.
2. Run `nix flake check --no-build` to catch evaluation errors before building.
3. Detect the platform:
   - **Linux** (desktop, laptop, server): run `nh os switch .`
   - **Darwin** (macbook-pro): run `nh darwin switch .`
4. **Never** run `nix flake update` unless the user explicitly asks for it.
5. On build failure:
   - Show `journalctl -xe -b` (Linux) or the darwin build log output
   - Suggest rollback: `nixos-rebuild switch --rollback` (Linux) / `darwin-rebuild switch --rollback` (Darwin)
   - Run `nix flake check --no-build --show-trace` for detailed evaluation errors
