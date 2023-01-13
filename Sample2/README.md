# Sample 2: GitOps repository to support both real-time and reconciliation window changes

This sample is part of the post []() and illustrates how one can structure the GitOps repository to manage real-time changes of some application resources, as well as having some application resources be managed by a reconciliation window.

## Setup of Flux resources

### 1. Create `Source` resource

```sh
flux create source git source \
    --url="https://github.com/<github-handle>//flux-reconciliation-windows-sample" \
	--username=<username> \
    --password=<PAT-token> \
    --branch=main \
    --interval=1m \
    --git-implementation=libgit2 \
    --silent
```

### 2. Create `Kustomization` resource for infra

```sh
flux create kustomization infra \
    --path="./Sample2/clusters/cluster-1/infra/"  \
    --source=source\
    --prune=true \
    --interval=1m
```

### 3. Create `Kustomize` resource for apps

```sh
flux create kustomization apps-rt \
    --path="./Sample2/clusters/cluster-1/apps/apps-rt" \
    --depends-on=infra \
    --source=source\
    --prune=true \
    --interval=1m
```

Note that the real-time kustromization will be reconciled before the one managed by the reconciliation window.
The main reason for this is if the order wourld be different (i.e. `apps-rt` depends on `apps-rw` ) whenever the `apps-rw` `Kustomize` controller is suspended the reconciliation of the `apps-rt` `Kustomize` controller would never be triggered.

```sh
flux create kustomization apps-rw \
    --depends-on=apps-rt \
    --path="./Sample2/clusters/cluster-1/apps/apps-rw" \
    --source=source\
    --prune=true \
    --interval=1m
```

## 3. Customize reconciliation windows times

> Note: In this sample changes to the reconciliation window definition will also only be applied within the reconciliation window itself.

To change the schedule for each cluster individually, the cron-string can be patched like the example `start-patch.yaml`/`stop-patch.yaml` under `/clusters/cluster-1/infra/reconciliation-window`.
Edit the string in `spec.schedule` to reflect when you want to have your reconciliation window open/close.
