---
description: "Generate new NixOS modules with proper structure and best practices"
---

# Create Module

Generate a new NixOS module. Ask the user for the module name and purpose, then follow this process.

## Determine category

Place the module based on the repo structure:

| Directory | Purpose | Notes |
|---|---|---|
| `modules/core/` | System fundamentals | boot, fonts, locale |
| `modules/shared/` | Cross-platform | NixOS + darwin, platform detection required |
| `modules/services/` | Systemd services | NixOS-only |
| `modules/home/` | Home Manager (user-level) | Cross-platform. **Server does NOT use HM** |
| `modules/virtualization/` | VM/container config | |
| `modules/gaming/` | Gaming-related | |
| `modules/desktop/` | Desktop environment | |
| `modules/hardware/` | Hardware-specific | |
| `modules/networking/` | Network config | |
| `modules/packages/` | Package sets | |

## Module archetypes

### NixOS-only (modules/core/, modules/services/, etc.)
```nix
{
  config,
  lib,
  pkgs,
  ...
}: let
  cfg = config.modules.<category>.<name>;
in {
  options.modules.<category>.<name> = {
    enable = lib.mkEnableOption "<description>";
  };

  config = lib.mkIf cfg.enable {
    # implementation
  };
}
```

### Cross-platform (modules/shared/)
```nix
{
  config,
  lib,
  pkgs,
  options,
  ...
}: let
  cfg = config.modules.shared.<name>;
  isNixOS = builtins.hasAttr "fileSystems" options;
in {
  options.modules.shared.<name> = {
    enable = lib.mkEnableOption "<description>";
  };

  config = lib.mkIf cfg.enable {
    # use lib.mkIf isNixOS { ... } for platform-specific blocks
  };
}
```

### Home Manager (modules/home/)
```nix
{
  config,
  lib,
  pkgs,
  ...
}: {
  # Uses programs.* or home.* options directly
  # No enable option needed if always active
}
```

## Rules

- Use alejandra formatting: 2-space indent, trailing commas.
- Always use explicit `lib.mkIf`, `lib.mkEnableOption`, `lib.mkOption` — never `with lib;`.
- Use `lib.optionals` for conditional list items, `lib.optionalAttrs` for conditional attrsets.
- Register the new module in the appropriate `default.nix` imports file.
- Write the file, then verify with `nix flake check --no-build`.
