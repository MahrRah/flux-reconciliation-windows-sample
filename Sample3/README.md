# `flux-maintanance-windows-sample`

The sample for edge-01 shows how an edge device is only managed during a reconciliation window.

## 1. Create `Source` controler

```sh
flux create source git source \
    --url="https://github.com/<github-handle>/flux-maintanance-windows-sample" \
	--username=<username> \
    --password=<PAT-token> \
    --branch=master \
    --interval=1m \
    --git-implementation=libgit2 \
    --silent
```

## 2. Create `Kustomize` controler

```sh
flux create kustomization bootstrap \
    --path="./Sample3/flux/" \
    --source=source\
    --prune=true \
    --interval=1m
```

```sh
flux create kustomization apps \
    --depends-on=infra \
    --path="./Sample1/clusters/edge-01/apps/" \
    --source=source\
    --prune=true \
    --interval=1m
```

## 3. Customize reconciliation windows times

> Note: In this sample changes to the reconciliation window definition will also only be applied within the reconciliation window itself.

To change the schedule for each cluster individually, the cron-string can be patched like the example `start-patch.yaml`/`stop-patch.yaml` under `/clusters/cluster-1/infra/reconciliation-window`.
Edit the string in `spec.schedule` to reflect when you want to have your reconciliation window open/close.
