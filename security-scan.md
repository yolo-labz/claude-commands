---
description: "NixOS security analysis and vulnerability assessment"
---

# Security Scan

Perform each of these concrete checks against the NixOS configuration. Report findings with severity (critical/warning/info).

## Secrets management
- Verify every sensitive value (API keys, passwords, tokens) lives in `secrets/` and is managed by sops-nix.
- Search for plaintext secrets: `grep -rn 'password\|secret\|token\|api.key' --include='*.nix' . | grep -v sops | grep -v mkOption`.
- Check nix store for leaked secrets: `nix-store --query --references /run/current-system | xargs grep -rl 'password' 2>/dev/null`.

## Firewall
- Audit `networking.firewall.enable` is `true` on all hosts.
- List all `allowedTCPPorts` and `allowedUDPPorts` — flag any unexpected open ports.
- Check for `networking.firewall.allowedTCPPortRanges` that are too broad.

## Service hardening
- For each systemd service in the config, check these options are set where appropriate:
  - `DynamicUser`, `ProtectSystem = "strict"`, `ProtectHome = true`
  - `NoNewPrivileges = true`, `PrivateDevices = true`, `PrivateTmp = true`
- Flag services running as root without hardening.

## SSH configuration
- Verify `services.openssh.settings.PermitRootLogin = "no"`.
- Verify `services.openssh.settings.PasswordAuthentication = false`.
- Check for any `authorizedKeys` entries that look stale or unexpected.

## Nix daemon
- Check `nix.settings.allowed-users` — should not be `["*"]`.
- Check `nix.settings.trusted-users` — only `root` and explicitly trusted users.

## Audit logging
- Verify `security.auditd` is either disabled or has proper size/rotation limits.
- **Critical**: previous incidents caused 289GB audit logs. If enabled, confirm `security.audit.rules` and log rotation are configured.

## File permissions
- Check for world-readable sensitive files: `find /etc -perm -004 -name '*secret*' -o -name '*key*' -o -name '*password*' 2>/dev/null`.

## Flake inputs
- Review `flake.lock` modification dates — flag inputs not updated in 90+ days.
- Check if any inputs track `master`/`main` instead of a release branch.
