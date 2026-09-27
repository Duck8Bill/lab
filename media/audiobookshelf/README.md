# Audiobookshelf

## What it is
Self-hosted audiobook and podcast server — library management, playback,
progress tracking across devices.

## Why I added it
Audiobook hosting

## Deployment  [audiobookshelf]
- **Method**: Helm, chart `audiobookshelf` (depends on the shared `common`
  library chart), from the `geek-cookbook` repo
  (`https://geek-cookbook.github.io/charts/`)
- **Namespace**: `audiobookshelf`
- **Exposed via**: Traefik IngressRoute — `audiobookshelf.codexasystems.net`
  , existing wildcard TLS cert
- **Storage**: [confirm — audiobook library PVC, metadata/progress DB location]

## Configuration notes


## How to deploy
\```bash
kubectl create namespace audiobookshelf
helm install audiobookshelf -n audiobookshelf -f audiobookshelf-values.yaml
\```

## Gotchas / issues hit
- 

## GitOps
Converted to ArgoCD management — clean, no issues with the conversion
itself. Minor follow-up: an early Application manifest had a typo in
`metadata.name` (`audiobbokshelf`), briefly creating a duplicate
Application tracking the same live resources. Resolved via a non-cascade
delete of the typo'd Application — no impact to the running app or its data.

## Status
- [x] Running stable
- [x] TLS configured
- [x] ArgoCD-managed
- [ ] Backed up / persistent data protected — [confirm status]
- [ ] Monitored — general cluster-wide gap, not unique to this app
