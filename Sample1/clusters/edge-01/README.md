# `flux-maintanance-windows-sample`

The sample for edge-01 shows how an edge device is only managed during a maintenance window.


## Create `Source` controler

```sh
flux create source git source \
    --url="https://github.com/MahrRah/flux-maintanance-windows-sample" \
	  --username=<username> \
    --password=<PAT-token> \
    --branch=master \
    --interval=1m \
    --git-implementation=libgit2 \
    --silent
```
## Create `Kustomize` controler



```sh
flux create kustomization infra \
    --path="./Sample1/clusters/edge-01/infra/" \
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

## Use Maintenance windows

Changes to the maintenance window will also be only applied with in a maintenance widnow itself.

To change the schedule, the cron-string in the patches `clusters/edge-01/infra/start-patch.yaml` can be adapted for each device.
