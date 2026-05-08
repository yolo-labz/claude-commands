# claude-commands

Custom slash commands for [Claude Code](https://claude.com/claude-code), consumed as the `commands/` source directory of a home-manager `programs.claude-code` configuration.

## Commands

| Command | Purpose |
|---|---|
| `/create-module` | Scaffold a new NixOS or Home Manager module |
| `/debug-config` | Walk through a NixOS config diagnostic |
| `/dream` | Open-ended brainstorm with explicit reasoning |
| `/nix-rebuild` | Run `nh os switch` with safety checks |
| `/optimize-performance` | Profile and remediate performance issues |
| `/security-scan` | Run security audit (OSV, secrets, perms) |
| `/speak` | Speak via local TTS (kokoro-speakd integration) |
| `/test-strategy` | Generate a test plan for the current change |

## Consumption from Nix

```nix
fetchFromGitHub {
  owner  = "yolo-labz";
  repo   = "claude-commands";
  rev    = "v0.1.0";
  sha256 = "<nix-prefetch-github yolo-labz claude-commands --rev v0.1.0>";
}
```

Reference from `programs.claude-code.commandsDir` (or equivalent) in your home-manager config.

## Adding a command

Each `.md` file at the repo root becomes one slash command. Filename → command name. Optional YAML frontmatter (model, allowed-tools, description) is honored by Claude Code.

## Versioning

Conventional Commits + semver. Releases use `actions/attest-build-provenance@v2` for SLSA Level 3 provenance.

## License

MIT — see [`LICENSE`](LICENSE).
