# `flux-maintanance-windows-sample`

The sample for edge-02 shows how an edge device could be managed using live changes as well as changes during a maintenance window.

# Create `Source` controller

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

## Create `Kustomize` controller
```sh
flux create kustomization infra-rt \
    --path="./Sample2/clusters/edge-02/infra/infra-rt"  \
    --source=source\
    --prune=true \
    --interval=1m
```

```sh
flux create kustomization apps-rt \
    --depends-on=infra \
    --path="./Sample2/clusters/edge-02/apps/apps-rt" \
    --source=source\
    --prune=true \
    --interval=1m
```

```sh
flux create kustomization apps-poc \
  --depends-on=infrastructure-poc \
  --path="./clusters/apps-live/" \
  --source=config-poc \
  --prune=true \
  --interval=1m
```

```sh
flux create kustomization apps-poc \
  --depends-on=infrastructure-poc \
  --path="./clusters/apps-live/" \
  --source=config-poc \
  --prune=true \
  --interval=1m
```

## Use Maintenance windows

This sample demonstrates how the cronjobs could be setup to be able to manage the schedule the maintenance window at any given moment.

Similar as for the other sample, to change the schedule the cron-string in the patches `clusters/edge-01/infra-live/start-patch.yaml` can be adapted for each device.