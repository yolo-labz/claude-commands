---
description: "Smart NixOS system rebuild with host detection and validation"
---

# Nix Rebuild

1. Run `git status` in the flake directory. Warn if there are uncommitted changes — unstaged files won't be included in the build.
2. Run `nix flake check --no-build` to catch evaluation errors before building.
3. Detect the platform:
   - **Linux** (desktop, laptop, server): run `nixos-smart-switch` when it is on
     `PATH`, otherwise `nh os switch .`. Live activation restarts the systemd
     **user** manager, which takes a running Wayland compositor session — and
     every process inside it — down with it; the wrapper builds, dry-activates,
     and falls back to `nh os boot` when the new generation would restart a
     session-critical unit. On a headless host the two are equivalent.
   - **Darwin** (macbook-pro): run `nh darwin switch .`
4. **Never** run `nix flake update` unless the user explicitly asks for it.
5. On build failure:
   - Show `journalctl -xe -b` (Linux) or the darwin build log output
   - Suggest rollback: `nixos-rebuild boot --rollback` + reboot (Linux) /
     `darwin-rebuild switch --rollback` (Darwin). `boot` for the same reason as
     step 3 — `nixos-rebuild switch --rollback` activates live, so the rollback
     itself can drop the session you are rolling back to save.
   - Run `nix flake check --no-build --show-trace` for detailed evaluation errors
