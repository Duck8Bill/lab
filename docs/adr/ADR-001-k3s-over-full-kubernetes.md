# ADR-001: K3s over Full Kubernetes

## Status
Accepted

## Context
Needed a Kubernetes distribution to run on two repurposed laptops with
limited RAM and no dedicated hardware. Options considered were full
upstream Kubernetes (kubeadm), K3s, and MicroK8s.

## Decision
Chose K3s. It ships with sensible defaults for a small cluster —
Traefik as ingress, ServiceLB (Klipper) for LoadBalancer IPs, local-path
provisioner for storage — reducing the number of components to install
and manage manually. Single binary install, low memory footprint, and
actively maintained by Rancher/SUSE.

## Consequences
- Cluster running within an hour of first install
- Traefik, cert-manager integration, and local-path storage all worked
  without additional configuration
- K3s-specific components (Klipper, Traefik addon) occasionally differ
  from upstream Kubernetes behaviour — requires awareness when following
  generic Kubernetes documentation
- Skills transfer directly to full Kubernetes — K3s is conformant
