# Auto-upgrading nodes

## Checking the state of auto-upgrade for an existing node pool

```
gcloud container node-pools describe node-pool-name \
  --cluster cluster-name \
  --zone compute-zone
```

## Enabling node auto-upgrades for an existing node pool

```
gcloud container node-pools update node-pool-name --cluster cluster-name \
    --zone compute-zone --enable-autoupgrade
```

## Disabling node auto-upgrades for an existing node pool

where:

```
gcloud container node-pools update node-pool-name --cluster cluster-name \
    --zone compute-zone --no-enable-autoupgrade
```

- node-pool-name is the name of the node pool.
- cluster-name is the name of the cluster that contains the node pool.
- compute-zone is the zone for the cluster.

