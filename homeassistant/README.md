# Home Assistant

## What it is
Self-hosted home automation platform — deployed via Helm to match the rest
of the homelab stack. Currently light on actual automations (a few thermostats,
Meshtastic node telemetry under consideration) but primarily added as the
anchor for an end-to-end CI/CD pipeline demonstration.

## Why I added it
Two reasons. First, it fills an obvious gap in the stack for a home with
some smart devices. Second, and more deliberately: it was the vehicle for
adding a proper CI pipeline to the repo — see the CI/CD section below.

## Deployment
- **Chart**: `pajikos/home-assistant` — `0.3.82`
- **Namespace**: `homeassistant`
- **Exposed via**: Traefik IngressRoute — `ha.codexasystems.net`
- **Storage**: 5Gi PVC via default `local-path` StorageClass — HA config,
  automations, and integrations. Not on NFS (no strong reason to be,
  config data is small and local-path is simpler here).

## CI/CD — the real reason this app is here

This was the first app deployed through a complete CI/CD pipeline rather
than a direct commit to main:

1. **Branch** — changes developed on `feature/homeassistant-cicd`
2. **CI** — GitHub Actions workflow on PR:
   - `helm template` renders the values file against the upstream chart
   - `kubeconform` validates rendered manifests against Kubernetes schemas
   - PR check must pass before merge is allowed
3. **Merge to main** — ArgoCD detects the change and syncs
4. **ArgoCD** — deploys from main, `selfHeal: true`, `prune: false`

The workflow lives at [`.github/workflows/helm-lint.yaml`](../.github/workflows/helm-lint.yaml).
It covers all apps in the repo, not just Home Assistant.

## Gotchas / issues hit
- **Namespace must exist before ArgoCD can sync** — ArgoCD will show
  `Missing` indefinitely if the namespace isn't created first.
  `kubectl create namespace homeassistant` is a manual prerequisite.
  Could be resolved with `CreateNamespace=true` in the ArgoCD sync policy
  but not done here — deliberate, keeps the manual step visible.
- **Slow first boot** — HA does significant first-run setup (default config,
  integration discovery). Readiness/liveness probes fire before it's ready.
  Not a misconfiguration — just needs `initialDelaySeconds` increased if
  probe failures become a problem in practice.
- **targetRevision on ArgoCD Application must match the branch in use** —
  during development the values source pointed to `feature/homeassistant-cicd`;
  after merging, this must be updated to `main` or ArgoCD pulls from a
  deleted branch. Updated as part of the merge PR.

## Status
- [x] Running and accessible via Traefik IngressRoute
- [x] Deployed via full CI/CD pipeline
- [ ] Meaningful automations — minimal at present
- [ ] Secrets via Sealed Secrets — not yet needed, no credentials in values
- [ ] Monitored — no HA-specific metrics scraped yet
