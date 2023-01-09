# `flux-maintanance-windows-sample`

The sample for edge-01 shows how an edge device is only managed during a maintenance window.

## Create `kustomization` resources

```sh
flux create kustomization infrastructure-poc \
  --path="./clusters/pump1/infra" \
  --source=config-poc \
  --prune=true \
  --interval=1m
```

```sh
flux create kustomization apps-poc \
  --depends-on=infrastructure-poc \
  --path="./clusters/apps/" \
  --source=config-poc \
  --prune=true \
  --interval=1m
```

## Use Maintenance windows

Changes to the maintenance window will also be only applied with in a maintenance widnow itself.

To change the schedule, the cron-string in the patches `clusters/edge-01/infra/start-patch.yaml` can be adapted for each device.
