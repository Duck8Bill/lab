# ADR-003: Deliberate Sync Policy Progression

## Status
Accepted

## Context
ArgoCD supports automated sync with selfHeal and prune options. The
question was whether to enable these immediately or build confidence
gradually.

## Decision
Started with fully manual sync (selfHeal: false, prune: false) for all
apps. Moved to selfHeal: true once each app proved stable under GitOps.
Prune remains false cluster-wide — a resource being removed from git
should not automatically disappear from the cluster without explicit
confirmation.

## Consequences
- The homepage outage validated this approach — ArgoCD correctly reverted
  to the repo state, which happened to be wrong. With prune enabled the
  consequences could have been worse
- selfHeal: true has proven stable across all apps and is now the default
- prune: false remains a deliberate choice, not an oversight — manual
  confirmation before destructive operations is appropriate at this scale
