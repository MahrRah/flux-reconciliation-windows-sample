# Sample 3: GitOps repository with a "bootstrap" `Kustomization` approach

The sample contains a cluster that can be setup using a "bootstrap" `Kustomization` resource.

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

## 2. Create bootstrap `Kustomization` resource

```sh
flux create kustomization bootstrap \
    --path="./Sample3/flux/" \
    --source=source\
    --prune=true \
    --interval=1m
```

This resource will now create the `apps` and `infra` `Kustomization` resource.
