---
description: "NixOS system performance analysis and optimization"
---

# Optimize Performance

Analyze the system and NixOS config for performance issues. Run these checks and report actionable findings.

## Boot time
- Run `systemd-analyze blame` and `systemd-analyze critical-chain` to identify slow units.
- Flag any service taking >5 seconds.

## Nix store
- Run `nix store gc --dry-run` to show reclaimable space.
- Count old generations: `nix profile history --profile /nix/var/nix/profiles/system | wc -l`.
- Suggest `nix-collect-garbage -d` if >20 generations exist.

## Memory
- Check if earlyoom or systemd-oomd is enabled and configured.
- Check zram/zswap config in `boot.kernel.sysctl` and `zramSwap.enable`.
- Review swap configuration and cgroup memory limits on services.

## Disk hazards
- Check `/var/tmp` size — nix-daemon uses this as TMPDIR during builds.
- Check `/var/log/audit/` size — **critical**: previous 289GB incidents. Flag if >1GB.
- Run `df -h /` and `df -h /nix` to verify free space.

## Build performance
- Check `nix.settings.substituters` — ensure cachix or other binary caches are configured.
- Verify `nix.settings.max-jobs` and `nix.settings.cores` are set appropriately for the host.
- Check if `ccache` is enabled for large C/C++ builds.

## Kernel tuning
- Review `boot.kernel.sysctl` for performance-relevant settings (vm.swappiness, net.core.somaxconn, fs.inotify.max_user_watches).
- Flag missing inotify watch limits if running file-watching tools.
