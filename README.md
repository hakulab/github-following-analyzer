# github-following-analyzer

[![CI](https://github.com/hakulab/github-following-analyzer/actions/workflows/ci.yml/badge.svg)](https://github.com/hakulab/github-following-analyzer/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Find accounts you follow that do not follow you back on GitHub.com, with an allowlist for intentional exceptions. The command uses your current GitHub CLI account and only reads GitHub data.

## Quick start

Install [Nushell](https://www.nushell.sh/) 0.116 or later and [GitHub CLI](https://cli.github.com/), or use [Nix](https://nixos.org/download/) with flakes enabled.

Authenticate GitHub CLI for GitHub.com:

```bash
gh auth login --hostname github.com
```

Run from this checkout:

```bash
./src/github-following-analyzer.nu
```

With Nix:

```bash
nix run . --
```

Use `--json` for JSON output or `--config PATH` to select a configuration file.

## Configuration

The command reads `./config.toml` from the current working directory if it exists. To add exemptions, copy the example and edit the ignored local file:

```bash
cp config.example.toml config.toml
```

```toml
allow_list = ["trusted-user"]
```

Use bare usernames without an `@` prefix. Matching is case insensitive.

## Output

Text output lists usernames and profile URLs. Use `--json` for structured output. Summaries and errors go to stderr.

Keep your configuration and reports private.

## Development

See [AGENTS.md](AGENTS.md) for project contracts and verification commands.

## License

Copyright (c) 2026 [Hakula](https://hakula.xyz). Licensed under the [MIT License](LICENSE).
