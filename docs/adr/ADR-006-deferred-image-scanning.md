# ADR-006: Deferred Image Scanning — Trivy Supply Chain Incident

## Status
Accepted

## Context
Image scanning was identified as a meaningful CI pipeline addition —
scanning container images for known CVEs on every PR before deployment.
Trivy (by Aqua Security) is the most widely used tool for this in the
Kubernetes ecosystem and was the natural candidate.

## Decision
Deferred adding Trivy to the CI pipeline following a significant supply
chain compromise in March 2026, in which threat actors injected
credential-stealing malware into official Trivy releases and GitHub
Actions by exploiting a stolen Personal Access Token. 75 version tags
of the `aquasecurity/trivy-action` repository were backdoored.

Grype (by Anchore) identified as the likely alternative — independent
of Aqua Security, open source, similar functionality. Not yet
implemented.

## Consequences
- No image scanning in the CI pipeline currently — a known gap
- Avoiding a recently compromised tool in the CI pipeline is the correct
  call regardless of homelab vs production context
- Will revisit once Grype is evaluated or Trivy's supply chain integrity
  is re-established with confidence
- References: CVE-2026-26189, Wiz Research writeup on TeamPCP attack
