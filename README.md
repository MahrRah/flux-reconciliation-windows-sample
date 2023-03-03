# `flux-reconciliation-windows-sample`

This repository contains samples to illustrate how to enable reconciliation windows for a K8s cluster managed by [Flux](https://fluxcd.io/).

## Pre-requisits

- Running cluster (AKS, K3s, etc)
- Installed Flux CLI

## Set up

Fork repository

```sh
git fork https://github.com/MahrRah/flux-reconciliation-windows-sample.git
```

For the next steps follow `README.md` for the sample you are interested in:

1. [`Sample1`](Sample1/README.md): GitOps repository that enables reconciliation windows
2. [`Sample2`](Sample2/README.md): GitOps repository to support both real-time and reconciliation window changes
3. [`Sample3`](Sample3/README.md): GitOps repository with a "bootstrap" `Kustomization` approach
4. [`Sample4`](Sample4/README.md): GitOps repository with a "bootstrap" `Kustomization` approach, notification controler and variable substitution.
