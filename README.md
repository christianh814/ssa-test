# Argo CD SSA Test

Just testing the SSA apply for Argo CD

## Install K8S

Using KIND

```shell
kind create cluster
```

## Install the RC

Argo CD v2.5+

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Test App

Apply the test app from this repo (this will be "out of band" from Argo CD to test patching)

```shell
kubectl apply -f \
https://raw.githubusercontent.com/christianh814/ssa-test/main/out-of-band.yaml
```

Check the pods

```shell
kubectl get pods -n test
```

You should see one pod.

```
NAME                           READY   STATUS    RESTARTS   AGE
welcome-php-689d9cfb5b-sm5fk   1/1     Running   0          77sshell
```

## SSA Test

The Argo CD app is targeting the [app](app) directory. There is only one, incomplete, file there. It looks like this

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: welcome-php
  namespace: test
spec:
  replicas: 3
```

By using [server side apply annotation](app/deployment-patch.yaml#L5), you only have to store the "delta" (or just what you want to change) in your repo.

Apply the Argo CD Application

```shell
kubectl apply -f \
https://raw.githubusercontent.com/christianh814/ssa-test/main/application-ssa.yaml
```

Check your pods now

```shell
kubectl get pods -n test
```

You should now have 3 replicas

```
NAME                           READY   STATUS    RESTARTS   AGE
welcome-php-689d9cfb5b-nqrqr   1/1     Running   0          16s
welcome-php-689d9cfb5b-rgbck   1/1     Running   0          16s
welcome-php-689d9cfb5b-sm5fk   1/1     Running   0          8m33s
```
