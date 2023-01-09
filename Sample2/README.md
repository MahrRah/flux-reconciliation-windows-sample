# `flux-maintanance-windows-sample`

This sample is part of the post []() and ilustrated how one can structure the GitOps repositroy to manage real-time changes of some application resource, as well as having some application resources be managed by a reconciliation window. 

## Setup of Flux resources

### 1. Create `Source` controller

```sh
flux create source git source \
    --url="https://github.com/<github-handle>//flux-maintanance-windows-sample" \
	--username=<username> \
    --password=<PAT-token> \
    --branch=master \
    --interval=1m \
    --git-implementation=libgit2 \
    --silent
```

### 2. Create `Kustomize` controller for infra

```sh
flux create kustomization infra \
    --path="./Sample2/clusters/cluster-1/infra/"  \
    --source=source\
    --prune=true \
    --interval=1m
```

### 3. Create `Kustomize` controller for apps

```sh
flux create kustomization apps-rt \
    --path="./Sample2/clusters/cluster-1/apps/apps-rt" \
    --depends-on=infra \
    --source=source\
    --prune=true \
    --interval=1m
```

```sh
flux create kustomization apps-mw \
    --depends-on=apps-rt \
    --path="./Sample2/clusters/cluster-1/apps/apps-mw" \
    --source=source\
    --prune=true \
    --interval=1m
```

## 3. Customize Maintenance windows times


> Note: In this sample changes to the maintenance window definition will also only be applied within the maintenance window itself.

To change the schedule for each cluster individually, the cron-string can be patched like the example `start-patch.yaml`/`stop-patch.yaml` under `/clusters/cluster-1/infra/maintenance-window`.
Edit the string in `spec.schedule` to reflect when you want to have your reconciliation window open/close.