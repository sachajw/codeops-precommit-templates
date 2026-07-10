# Git Pre-commit Framework

[Pre-commit.com](https://pre-commit.com/) — a framework for managing and maintaining multi-language pre-commit hooks.

This repo bundles a wide range of hook integrations into one superset config (`pre-commit-config.yaml`). Each block is self-contained — copy only the blocks you need into your own `.pre-commit-config.yaml`.

## Table of Contents

- [Pre-commit](#pre-commit)
  - [Aliases](#aliases)
  - [Install with Homebrew](#install-with-homebrew)
- [Bundled Hooks](#bundled-hooks)
  - [Pre-commit Hooks (baseline)](#pre-commit-hooks-baseline)
  - [GitGuardian (ggshield)](#gitguardian-ggshield)
  - [Hadolint](#hadolint)
  - [Ruff](#ruff)
  - [mdformat](#mdformat)
  - [Jira Smart Commits](#jira-smart-commits)
  - [Terraform](#terraform)
  - [Helm Docs](#helm-docs)
  - [Codespell](#codespell)
  - [Maven](#maven)
  - [Prettier](#prettier)
  - [MegaLinter](#megalinter)
  - [KubeConform for Helm Charts](#kubeconform-for-helm-charts)
  - [Frigate](#frigate)

## Pre-commit

A framework for managing and maintaining multi-language pre-commit hooks.

### Aliases

```shell
alias pcinstall='pre-commit install'
```

```shell
alias pcuninstall='pre-commit uninstall'
```

```shell
alias pcall='pre-commit run --all-files'
```

```shell
alias pcup='pre-commit autoupdate'
```

### Install with Homebrew

```shell
brew install pre-commit
```

## Bundled Hooks

> Versions below are the current pins in `pre-commit-config.yaml`. Bump them with `pre-commit autoupdate --config pre-commit-config.yaml` and update this list to match.

### Pre-commit Hooks (baseline)

[pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks) — pinned `v4.6.0`

The always-present baseline of generic file sanity checks:

- `check-xml`, `check-json` — validate config file syntax
- `end-of-file-fixer` — ensure files end with a newline
- `trailing-whitespace` — strip trailing whitespace
- `check-merge-conflict` — refuse conflict markers before they land

### GitGuardian (ggshield)

[GitGuardian ggshield](https://docs.gitguardian.com/ggshield-docs/integrations/git-hooks/pre-commit) — pinned `v1.28.0`

Secrets detection for non-human-identity (NHI) security. Discover, prioritize, and remediate leaked credentials at scale.

- Put the GitGuardian API key in an `.env` file in the repo (or your `.bashrc` / `.zshrc`):
- Remember to add `.env` to `.gitignore`

```shell
GITGUARDIAN_API_KEY=<crazy long string of numbers and letters>
```

### Hadolint

[hadolint-py](https://github.com/AleksaC/hadolint-py) — pinned `v2.14.0`

A Dockerfile linter, bundled here as a pip wheel so no separate binary install is needed. Currently ignores `DL3018` and `DL3025`.

### Ruff

[Ruff](https://github.com/astral-sh/ruff-pre-commit) — pinned `v0.11.10`

An extremely fast Python linter and formatter (replaces Flake8/Black/isort toolchains). Two hooks run over `python` and `pyi` files:

- `ruff-check --fix` — lint and auto-fix
- `ruff-format` — format

### mdformat

[mdformat](https://github.com/hukkin/mdformat) — pinned `0.7.22`

An opinionated Markdown formatter. Plugins enabled:

- `mdformat-gfm` — GitHub-Flavored Markdown support
- `mdformat-ruff` — apply Ruff to embedded Python code blocks
- `mdformat-black` — apply Black to embedded Python code blocks

### Jira Smart Commits

[auto-smart-commit](https://github.com/radix-ai/auto-smart-commit) — pinned `v1.0.3`

Automatically prefixes commit messages with the relevant Jira issue key, enabling Jira [smart commits](https://confluence.atlassian.com/bitbucket/using-smart-commits-298979931.html). Requires installing the `prepare-commit-msg` hook stage:

```shell
pre-commit install --hook-type prepare-commit-msg
```

### Terraform

[pre-commit-terraform](https://github.com/antonbabenko/pre-commit-terraform) — pinned `v1.108.0`

A battery-included set of Terraform/OpenTofu IaC hooks:

- `terraform_fmt` — format
- `terraform_validate` — `terraform validate`
- `terraform_tflint` — TFLint static analysis
- `terraform_tfsec` — Tfsec security scan
- `terraform_checkov` — Checkov policy/compliance scan
- `terrascan` — Terrascan compliance scan
- `terraform_docs` — generate Terraform module docs
- `tfupdate` — bump provider/terraform versions
- `infracost_breakdown` — cost estimation via Infracost (configured with hourly-cost and monthly-cost-diff thresholds, emitting `infracost_report.html`)

### Helm Docs

[Helm Docs](https://github.com/norwoodj/helm-docs) — pinned `v1.14.2`

A tool for automatically generating markdown documentation for Helm charts. Works out-of-the-box with the built-in default template.

`linter_values.yaml` example (optional — only needed when chart templates reference values that must resolve for docs to render):

```yaml
dockerRegistry: ".dkr.ecr.eu-central-1.amazonaws.com"
accountPrefix:
regionShort:
blueGreenValue: blue

clusterNameTemplate: |-
  {{ print .Values.regionShort "-" | trimPrefix "ec1-" }}{{ include "account.name" . }}-{{ .Values.blueGreenValue }}
accounts:
  infra:
    id: ""
  nonprod:
    id: ""
  preprod:
    id: ""
  prod:
    id: ""
  common:
    id: ""

stages:
  test: ""
    account: nonprod
  playground:
    account: infra

vault:
  address:

filebeat:
  image:
    registry:  .dkr.ecr.eu-central-1.amazonaws.com
    repository: tools/filebeat
    tag: 7.12.1

dynatrace:
  groupnamePrefix:
```

### Codespell

[Codespell](https://github.com/codespell-project/codespell) — pinned `v2.2.2`

A lightweight typo checker that scans source for common misspellings against a built-in dictionary.

### Maven

[pre-commit-maven](https://github.com/ejba/pre-commit-maven) — pinned `v0.3.3`

Java/JVM build hooks. Currently runs `maven clean compile` and `maven-spotless-apply` (code formatting via Spotless).

### Prettier

[Prettier](https://prettier.io/) — pinned `v3.0.0-alpha.4`

An opinionated multi-language code formatter. Additional plugins extend coverage beyond the core languages to Java, shell, TOML, and XML.

### MegaLinter

[MegaLinter](https://github.com/oxsecurity/megalinter) — pinned `v6.8.0`

An all-in-one multi-language linter runner by OX Security. Two tiers:

- `megalinter-incremental` — faster, less thorough, runs at the `commit` stage
- `megalinter-full` — slower, more thorough, runs at the `push` stage

### KubeConform for Helm Charts

[KubeConform for Helm Charts](https://github.com/jtyr/kubeconform-helm) — pinned `v0.1.17`

A Helm plugin and pre-commit hook that renders charts and validates the rendered manifests against the Kubernetes schema with [`kubeconform`](https://github.com/yannh/kubeconform). Currently targets Kubernetes `1.24.0`.

### Frigate

[Frigate](https://frigate.readthedocs.io/en/latest/) — pinned `v0.4.0`

A tool for automatically generating documentation for your Helm charts. It uses the chart's `Chart.yaml` and `values.yaml` to generate content in a markup language of your choice. Here it emits `README.rst` in RST format.
