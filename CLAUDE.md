# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-purpose catalog repo: one master **`pre-commit-config.yaml`** that bundles a wide range of pre-commit hook integrations (Terraform/IaC, Helm/Kubernetes, Python/Ruff, Markdown, security scanning, formatting, docs generation) into one superset config, plus a hand-written **`README.md`** describing the tools. Remote: `git@github.com:sachajw/codeops-precommit-templates.git`. There is no application code, no build system, and no CI — the YAML file *is* the artifact.

## The one convention that matters: section delimiters

`pre-commit-config.yaml` is organized into self-contained blocks under the top-level `repos:` list, each bracketed by comment markers:

```yaml
# Terraform start
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.108.0
    hooks: [ ... ]
# Terraform end
```

Present sections: `default config`, GitGuardian (`ggshield`), Hadolint, Ruff, mdformat, Jira (`auto-smart-commit`), Terraform, Helm (`helm-docs`), Codespell, Maven, Prettier, MegaLinter, KubeConform, Frigate. When adding, removing, or editing a tool, operate on the whole block between its `# <name> start` / `# <name> end` markers. The first block (`# default config start` … `end`) is special — it wraps the file header plus the always-present `pre-commit-hooks` baseline.

Each `rev:` is pinned to an exact tag with an inline comment linking to upstream docs — preserve that comment when bumping.

## Not the same thing as the local template system

This repo is a **standalone monolithic reference**, distinct from the active composable system at `~/.config/pre-commit/` (split `templates/*.yaml` fragments composed by the `pcc` shell function into a project's `.pre-commit-config.d/`, with a runtime hook deciding which config runs — see that directory's own CLAUDE.md). The two share lineage but are **not kept in sync**: pinned versions have diverged (e.g. this repo pins `pre-commit-hooks` v4.6.0 and uses `ggshield` for secrets; the local baseline uses v5.0.0 and `gitleaks` v8.30.1) and the section/template names don't map 1:1. Don't try to reconcile them unless explicitly asked — treat this file as its own self-contained config.

## Working here

Nothing to build or unit-test. The realistic operations on the file:

```bash
# Validate it parses + conforms to the pre-commit config schema (fast, no runtimes needed)
pre-commit validate-config pre-commit-config.yaml

# Bump every pinned rev in place to latest
pre-commit autoupdate --config pre-commit-config.yaml
```

Because the file is a superset, running it wholesale will fail in any environment missing tool-specific runtimes (terraform, helm, maven, wiz, …) — that's expected. In practice consumers copy only the blocks they need into their own `.pre-commit-config.yaml`, so keep each block self-contained and independently usable.

## Keep README in sync

`README.md` is **hand-maintained** here (unlike `~/.config/pre-commit/`, where docs are generated). It currently documents only a subset of the sections present (Frigate, GitGuardian, Helm Docs, KubeConform) — the config has several more. When adding/renaming/removing a section, update the README and its anchor list to match.

## Commits

Emoji-prefixed conventional commits following the global commitizen schema (see `~/.config/commitizen/cz.toml`): `⬆️ BUILD:` for rev bumps, `🐛 FIX:`, `📝 DOC:`, `✨ FEATURE:` for new hook sections, etc. Work lands on `main` via the user's `gap` AI-push alias. (Older history used `🛠 NEW:` — that style has been superseded.)

## Repo hygiene

Only `README.md` and `pre-commit-config.yaml` are tracked. The visible local `.codegraph/` (CodeGraph index) and `graphify-out/` (graphify analysis output) directories are tool artifacts covered by the global gitignore — never commit them.
