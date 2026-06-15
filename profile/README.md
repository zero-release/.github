# zero-release

**Zero-runtime-dependency semantic release automation for GitHub Actions.**

zero-release analyzes Conventional Commits, calculates the next SemVer version,
generates release notes, creates annotated Git tags, and runs explicit publish
or notification plugins. Its core is Bash, not JavaScript, for projects that
want automated releases in CI without installing a runtime dependency tree.

[![zero-release tests](https://github.com/zero-release/zero-release/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/zero-release/zero-release/actions/workflows/tests.yml)
[![Documentation](https://img.shields.io/badge/docs-zero--release.github.io-blue)](https://zero-release.github.io/)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-FE5196?logo=conventionalcommits&logoColor=white)](https://www.conventionalcommits.org/)
[![Runtime dependencies](https://img.shields.io/badge/runtime%20dependencies-zero-brightgreen)](https://github.com/zero-release/zero-release)

## Why zero-release?

- **Small release toolchain:** the core CLI runs with Bash, Git, and standard
  Unix tools.
- **Zero JavaScript in the core:** no Node.js runtime, npm install,
  `node_modules`, or bundled JavaScript dependency chain.
- **GitHub Actions friendly:** use it as a composite action with stable workflow
  inputs and outputs.
- **Conventional Commit releases:** `feat:`, `fix:`, `perf:`, and breaking
  changes drive SemVer decisions.
- **Safe previews:** pull request workflows default to dry-run, so teams can see
  the release that would happen without publishing anything.
- **Explicit plugins:** file changes, package publishing, GitHub Releases, and
  notifications run only when enabled.
- **No project config execution:** zero-release does not load `.releaserc` files
  or execute repository-owned configuration.

## Quick start

```yaml
name: release

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: zero-release/zero-release@v1
        id: release
        with:
          branches: main
          plugins: release-notes,changelog,package-json,github-release
```

This workflow reads the full Git history, calculates the next version, updates
release assets through the selected plugins, creates a tag, and publishes a
GitHub Release.

## Built-in plugins

| Plugin | Purpose |
|---|---|
| `release-notes` | Generates Markdown release notes |
| `changelog` | Writes release notes to `CHANGELOG.md` |
| `package-json` | Updates the top-level `package.json` version |
| `git-commit` | Commits changed release assets before tagging |
| `npm` | Publishes packages with npm Trusted Publishing |
| `github-release` | Creates a GitHub Release |
| `webhook`, `slack`, `gchat` | Sends release notifications |

Network behavior is opt-in and network plugins are skipped in dry-run mode.

## Projects

- [zero-release/zero-release](https://github.com/zero-release/zero-release) -
  the Bash CLI, GitHub Action, built-in plugins, tests, and documentation.
- [zero-release.github.io](https://zero-release.github.io/) - user guide,
  plugin documentation, CLI reference, and security notes.

## Useful links

- [Getting started](https://zero-release.github.io/guide/getting-started/)
- [GitHub Actions guide](https://zero-release.github.io/guide/github-actions/)
- [Plugins](https://zero-release.github.io/plugins/)
- [CLI reference](https://zero-release.github.io/reference/cli/)
- [Security model](https://zero-release.github.io/reference/security/)
