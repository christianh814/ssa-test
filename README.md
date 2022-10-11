# Argo CD SSA Test

Just testing the SSA apply for rc 2.5

## Install K8S

Using KIND

```shell
kind create cluster
```

## Install the RC

Argo CD v2.5 RC

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.5.0-rc1/manifests/install.yaml
```

## Test App

Apply the test app from this repo
