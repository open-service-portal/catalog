# Open Service Portal Catalog

This repository serves as the central catalog for Crossplane templates.

## Purpose
- Central registry of all template-* repositories
- Managed by Flux for automatic synchronization
- Each template provides XRDs and Compositions for infrastructure
- Defines clear ownership boundaries for platform resources

## Ownership Model

Resources in this catalog follow a clear ownership structure based on GitHub Teams:

- **Core Infrastructure** → `group:default/platform-team`
  - Namespaces, DNS, Clusters, Networking, Security
  - Foundation that all services build upon
  
- **Application Services** → `group:default/service-provider`
  - Business applications, APIs, Databases
  - Services that use core infrastructure

For detailed ownership guidelines, see [Ownership Guidelines](https://github.com/open-service-portal/portal-workspace/blob/main/docs/ownership-guidelines.md)

## Repository Structure

```
catalog/
├── templates/              # Template repository definitions
│   ├── core/              # Platform team owned (future)
│   └── services/          # Service provider owned (future)
├── kustomization.yaml     # Kustomize configuration for Flux
└── README.md             # This file
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
- `template-dns-record` - DNS record management
- `template-cloudflare-dnsrecord` - Cloudflare DNS integration
- `template-namespace` - Kubernetes namespace provisioning
- `template-whoami` - Demo application deployment
- `template-whoami-service` - Service composition example

## Current Templates

| Template | Description | Owner | Resources Created |
|----------|-------------|-------|-------------------|
| `template-namespace` | Kubernetes namespace management | platform-team | Namespace, ResourceQuota, NetworkPolicy, RBAC |
| `template-dns-record` | DNS record management (mock) | platform-team | DNS A/CNAME records for local testing |
| `template-cloudflare-dnsrecord` | Cloudflare DNS integration | platform-team | Real DNS records in Cloudflare zones |
| `template-whoami` | Demo application | service-provider | Deployment, Service, Ingress |
| `template-whoami-service` | Service composition | service-provider | Combines namespace + whoami app |

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