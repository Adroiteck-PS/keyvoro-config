# keyvoro-config

OKD deployment manifests for [Keyvoro CRM](https://github.com/Adroiteck-PS/keyvoro).

## Structure

```
keyvoro-config/
  base/                    # Base manifests (shared across environments)
    deployment.yaml        # No PEM, no secrets — refs ConfigMap by name
    service.yaml
    route.yaml
    networkpolicy.yaml
    rbac.yaml
    migration-job.yaml
    keyvoro-changelogs-cm.yaml
    kustomization.yaml
  overlays/
    production/            # Production overrides
      kustomization.yaml
    staging/               # Staging overrides
      kustomization.yaml
  externalsecrets/
    vault.yaml             # VaultStaticSecret → Vault
```

## Rules

- **No PEM content** in this repo (CA bundles managed by Ansible)
- **No secrets** in this repo (Vault via ExternalSecret/VSO)
- **No resources with `adroiteck-` prefix** (reserved for infra-managed)
- ArgoCD syncs from this repo, not the source code repo

## CA Trust Bundle

The `adroiteck-ca-bundle` ConfigMap is deployed by Ansible to the keyvoro namespace.
See `apps/CLAUDE.md` in the main workspace for the deployment pattern.

## Deployment

ArgoCD syncs automatically. Manual sync:
```bash
argocd app sync keyvoro
```
