# Open Service Portal Catalog

This repository serves as the central catalog for Crossplane templates.

## Purpose
- Central registry of all template-* repositories
- Managed by Flux for automatic synchronization
- Each template provides XRDs and Compositions for infrastructure

## Structure

```
catalog/
├── templates/        # Individual template repository definitions
├── kustomization.yaml  # Kustomize configuration for Flux
└── README.md        # This file
```

## Adding a Template

1. Create your template repository as `template-<name>` in the organization
2. Create a YAML file in the `templates/` directory with:
   - GitRepository resource pointing to your template repo
   - Kustomization resource to sync it to the cluster
3. Update kustomization.yaml to include your new file
4. Submit PR for review

## Template Naming Convention

All template repositories must follow the pattern: `template-<resource-type>`

Examples:
- `template-dns-record`
- `template-postgres-db`
- `template-kubernetes-app`

## How It Works

1. Flux watches this catalog repository
2. When new templates are added, Flux creates GitRepository resources
3. Each template repository contains:
   - XRD (Composite Resource Definition) - The API
   - Composition - The implementation
   - Examples - How to use it

## Checking Sync Status

```bash
# Check if catalog is syncing
flux get sources git catalog -n flux-system

# Check all template repositories
flux get sources git -n flux-system | grep template-

# Check Crossplane resources
kubectl get xrd
kubectl get compositions
```