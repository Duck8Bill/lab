# Changelog

## 2026-10-04
- Added Renovate bot for automated dependency updates
- Added branch protection rules on main
- Added CI status badge to README
- Disabled Docker service on control plane node (unused, DNS noise source)
- Added Technitium reverse DNS zones for pod/service CIDRs (10.42, 10.43)

## 2026-10-03
- Added GitHub Actions CI pipeline (helm template | kubeconform)
- Deployed Home Assistant via full CI/CD pipeline
- Added Traefik IngressRoute for ha.codexasystems.net
- Configured trusted proxy via HA UI for Traefik reverse proxy
- Updated main README with CI pipeline section and Mermaid diagram
- Added per-app README for Home Assistant

## Prior
- See git log and README for evolutionary history
