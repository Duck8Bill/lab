# ADR-005: Remote Helm Charts over Local

## Status
Accepted

## Context
Helm applications can be deployed using local chart directories checked
into the repo, or by referencing upstream community charts via remote
Helm repositories, with only a values file stored locally.

## Decision
Use remote upstream charts wherever available, storing only values files
in this repo. Charts are pinned to explicit versions via targetRevision
in ArgoCD Application manifests.

## Consequences
- Repo stays lean — only configuration lives here, not chart boilerplate
- Upstream chart updates are explicit version bumps, reviewed via PR
  (automated by Renovate bot)
- CI pipeline uses helm template against remote charts — requires
  helm repo add steps in GitHub Actions workflow
- helm lint does not work with remote charts (requires local Chart.yaml)
  — kubeconform against helm template output is used instead
- Full trust placed in upstream chart maintainers — mitigated by pinned
  versions and CI validation on every update
