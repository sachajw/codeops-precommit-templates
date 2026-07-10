[Git Pre-commit Framework](#git-pre-commit-framework)

- [Git Pre-commit Framework](#git-pre-commit-framework)
  - [Aliases](#aliases)
  - [Install Pre-commit with Homebrew](#install-pre-commit-with-homebrew)
- [Frigate](#frigate)
- [GitGuardian](#gitguardian)
- [Helm Docs](#helm-docs)
- [KubeConform for Helm Charts](#kubeconform-for-helm-charts)

### Git Pre-commit Framework

[Pre-commit.com](https://pre-commit.com/)

A framework for managing and maintaining multi-language pre-commit hooks.

#### Aliases

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

#### Install Pre-commit with Homebrew

```shell
brew install pre-commit
```

### Frigate

[Frigate](https://frigate.readthedocs.io/en/latest/)

Frigate is a tool for automatically generating documentation for your Helm charts.
It will use the chart’s Chart.yaml and values.yaml files in order to generate the content in a markup language of your choice.

### GitGuardian

[GitGuardian.com](https://www.gitguardian.com/)

Take Control Of Your NHI Security. Discover all your secrets. Prioritize and remediate leaks at scale. Protect your non-human identities and reduce breach risks.

- `.env` file in the repo for scanning with the GitGuardian API key
- Remember to add `.env` to `.gitignore`

```shell
GITGUARDIAN_API_KEY=<crazy long string of numbers and letters>
```

### Helm Docs

[Helm Docs](https://github.com/norwoodj/helm-docs)

A tool for automatically generating markdown documentation for helm charts

`linter_values.yaml` example (optional — only needed when chart templates reference values that must resolve for docs to render; helm-docs works out-of-the-box with the built-in default template otherwise)

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

### KubeConform for Helm Charts

[KubeConform for Helm Charts](https://github.com/jtyr/kubeconform-helm)

This repo contains tools that allow to use kubeconform to test Helm charts in the form of Helm plugin and pre-commit hook.