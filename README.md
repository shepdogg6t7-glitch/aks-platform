# AKS Platform

This repository contains the Terraform configuration for deploying a private Azure Kubernetes Service (AKS) cluster.

## Architecture
- **Network:** VNet with a /24 subnet, CNI Overlay enabled.
- **AKS Cluster:** Private cluster, system node pool, workload pool scaling to zero.
- **Security:** Workload Identity, OIDC issuer, local accounts disabled.
- **Budget:** Separate root in `bootstrap/budget` to prevent unexpected costs.

## Cost Warning

The idle floor is ~$97/month. The default budget is set to $150/month.

## Verify

Three things gate `kubectl` here, all of them deliberate:

- `private_cluster_enabled = true`: the API server has no public endpoint, so a workstation needs a route into the VNet.
- `local_account_disabled = true`: there is no client certificate to fall back on, so every call goes through Entra and `--admin` does not exist.
- `admin_group_object_ids` defaults to empty, so even a valid token returns 403 until something is granted `Azure Kubernetes Service RBAC Cluster Admin` on the cluster.

With no network path at all, run the command server-side:

```bash
az aks command invoke \
  --resource-group rg-aks-dev --name aks-dev \
  --command "kubectl get nodes -o wide"
  