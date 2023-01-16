# Sample 1: GitOps repository that enables reconciliation windows

This sample is part of the blog post ["How to enable reconciliation windows using Flux and K8s native components"](https://dev.to/mahrrah/how-to-enable-reconciliation-windows-using-flux-and-k8s-native-components-2d4i) and illustrates how one can leverage Flux and K8s CronJobs to manage reconciliations to happen only in a designated time window called "reconciliations window".

## 1. Create `Source` resource

```sh
flux create source git source \
    --url="https://github.com/<github-handle>/flux-reconciliation-windows-sample" \
	--username=<username> \
    --password=<PAT-token> \
    --branch=main \
    --interval=1m \
    --git-implementation=libgit2 \
    --silent
```

## 2. Create `Kustomization` resource

```sh
flux create kustomization infra \
    --path="./Sample1/clusters/cluster-1/infra/" \
    --source=source\
    --prune=true \
    --interval=1m
```

```sh
flux create kustomization apps \
    --depends-on=infra \
    --path="./Sample1/clusters/cluster-1/apps/" \
    --source=source\
    --prune=true \
    --interval=1m
```

## 3. Customize reconciliation windows times

> Note: In this sample changes to the reconciliation window definition will also only be applied within the reconciliation window itself.

To change the schedule for each cluster individually, the cron-string can be patched like the example `start-patch.yaml`/`stop-patch.yaml` under `/clusters/cluster-1/infra/reconciliation-window`.
Edit the string in `spec.schedule` to reflect when you want to have your reconciliation window open/close.
