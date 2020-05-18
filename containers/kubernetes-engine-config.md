# Kubernetes Engine Config

Create Clusters

```
gcloud container clusters create foo
gcloud container clusters create bar
```

Get Credentials

```
gcloud container clusters get-credentials foo
gcloud container clusters get-credentials bar
```

View Kubernetes Config

```
kubectl config view
```

The config is located in

```
~/.kube/config
```

Switch between clusters

```
kubectl config use-context foo
Switched to context "foo"

kubectl config use-context bar
Switched to context "bar"
```