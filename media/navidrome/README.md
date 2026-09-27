# Navidrome

## What it is
Self-hosted music streaming server — subsonic-compatible, browser/app-based
access to a personal music library.

## Why I added it
To host music library locally and maintain digital independence

## Deployment
- **Method**: Helm, chart `navidrome` from the `djjudas21` repo
  (`https://djjudas21.github.io/charts/`)
- **Namespace**: media
- **Exposed via**: Traefik IngressRoute — `music.codexasystems.net, 
  existing wildcard TLS cert
- **Storage**: nfs share for media only.

## Configuration notes

## Gotchas / issues hit
The Application's `chart:` field takes the bare chart name (`navidrome`)

## GitOps
Converted to ArgoCD management. One gotcha worth remembering: the
Application's `chart:` field takes the bare chart name (`navidrome`), not
the Helm-CLI-style `repoalias/chartname` (`djjudas21/navidrome`) — the repo
alias is redundant once `repoURL` is already specified separately, and
including it gets parsed as if it were the chart's actual name, which
doesn't exist under that string.

## Status
- [x] Running stable
- [x] TLS configured
- [x] ArgoCD-managed
- [ ] Backed up / persistent data protected — [confirm status]
- [ ] Monitored — general cluster-wide gap, not unique to this app
