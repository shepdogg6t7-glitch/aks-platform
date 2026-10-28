@"
# AKS Platform

This repository contains the Terraform configuration for deploying a private Azure Kubernetes Service (AKS) cluster.

## Architecture
- **Network:** VNet with a /24 subnet, CNI Overlay enabled.
- **AKS Cluster:** Private cluster, system node pool, workload pool scaling to zero.
- **Security:** Workload Identity, OIDC issuer, local accounts disabled.
- **Budget:** Separate root in `bootstrap/budget` to prevent unexpected costs.

## Cost Warning
The idle floor is ~$97/month. The default budget is set to $150/month.
"@ | Out-File -FilePath "README.md" -Encoding utf8