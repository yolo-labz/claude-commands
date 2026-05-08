# Security Policy

## Supported Versions

Only the latest tagged release of `claude-commands` receives security updates.
Pre-release/development builds from `main` are best-effort.

| Version  | Supported          |
| -------- | ------------------ |
| latest   | :white_check_mark: |
| < latest | :x:                |

## Reporting a Vulnerability

**Please do NOT open public GitHub issues for security vulnerabilities.**

Use one of these private channels:

1. **GitHub Security Advisories (preferred)** — open a private advisory at
   https://github.com/yolo-labz/claude-commands/security/advisories/new
2. **Email** — contact the maintainer directly via the email listed on
   https://github.com/phsb5321

### What to include

- Affected version (commit SHA or release tag)
- Reproduction steps or proof-of-concept
- Impact assessment (what data/system is at risk)
- Suggested mitigation (optional)

### Response SLA

- **Acknowledgement:** within 72 hours
- **Triage + initial assessment:** within 7 days
- **Fix or mitigation:** target 30 days for high/critical, 90 days for medium/low

We will credit reporters in the release notes unless anonymity is requested.

## Verifying Releases

Every release is published with cryptographic provenance via Sigstore.
Verify a downloaded release artifact:

```bash
gh attestation verify <artifact> --repo yolo-labz/claude-commands
```

SBOMs (CycloneDX 1.7 + SPDX 2.3) are attached to each GitHub Release for
supply-chain auditing.

## Threat Model

`claude-commands` is a flat collection of Markdown slash-command definitions
for Claude Code. It executes no code at install time and ships no runtime
binary. Risk surface is limited to:

- Malicious slash-command markdown that, when invoked, instructs the Claude
  agent to run dangerous shell commands (mitigated by reviewing diffs of
  any update before consumption — every command's body is plain Markdown).
- Tampered release tarball (mitigated by `gh attestation verify` + Sigstore
  provenance + SHA256 pinning in downstream Nix consumers).

Out-of-scope:
- The runtime behavior of Claude Code itself or any commands the agent
  decides to execute.
- Downstream consumer security (Nix flakes pinning this repo by SHA are
  responsible for their own update review process).
