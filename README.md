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

Apply the test app from this repo (this will be "out of band" from Argo CD to test patching)

```shell
kubectl apply -f \
https://raw.githubusercontent.com/christianh814/ssa-test/main/out-of-band.yaml
```

Check the pods

```shell
kubeclt get pods -n test
```

You should see one pod.

```
NAME                           READY   STATUS    RESTARTS   AGE
welcome-php-689d9cfb5b-sm5fk   1/1     Running   0          77sshell
```

## SSA Test


