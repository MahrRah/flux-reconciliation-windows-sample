# `flux-maintanance-windows-sample`

This repository contains samples to illustrate how to enable reconciliation windows for a K8s cluster managed by [Flux](https://fluxcd.io/).

## Pre-requisits

- Running cluster (AKS, K3s, etc)
- Installed Flux CLI

## Set up

Fork repository

```sh
git fork https://github.com/MahrRah/flux-maintanance-windows-sample.git
```

For the next steps follow `README.md` for the sample you are interested in:

1. [`Sample1`](Sample1/README.md): Updates only possible to cluster during a reconciliation window
2. [`Sample2`](Sample2/README.md): Applications have configurations that need to be updated in real-time as well as during reconciliation windows.
