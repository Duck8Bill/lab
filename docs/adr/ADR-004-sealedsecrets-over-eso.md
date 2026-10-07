# ADR-004: SealedSecrets over External Secrets Operator

## Status
Accepted

## Context
GitOps requires secrets to be stored somewhere. Options considered:
- SealedSecrets — encrypts secrets for safe git storage, decrypted by
  a controller running in the cluster
- External Secrets Operator (ESO) — pulls secrets from an external vault
  (HashiCorp Vault, AWS Secrets Manager, etc.) at runtime

## Decision
Chose SealedSecrets. No external secret store infrastructure required —
secrets live encrypted in the git repo alongside the manifests that use
them. Simple operational model for a single-operator cluster.

## Consequences
- Secrets are self-contained in the repo — no dependency on an external
  vault service being available at deploy time
- Private key backup is critical — if the SealedSecrets controller key
  is lost, all sealed secrets become unreadable. Key backed up to NAS
- ESO would be the right choice if secrets needed to be shared across
  multiple clusters or managed by a separate team
- Rotating a sealed secret requires re-sealing and committing — slightly
  more friction than ESO but acceptable at this scale
