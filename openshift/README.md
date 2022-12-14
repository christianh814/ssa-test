# OpenShift GitOps

With OpenShift GitOps 1.7+ Server Side Apply is supported.


## Demo with Router

Router, by default, has a scale of `2`

```shell
$ oc get pods -n openshift-ingress
NAME                              READY   STATUS    RESTARTS      AGE
router-default-766cf577f4-qk2qh   1/1     Running   1 (77m ago)   86m
router-default-766cf577f4-sd8p4   1/1     Running   1 (76m ago)   86m
```

In earlier versions, you had to get the YAML from the cluster

```shell
oc get ingresscontroller/default -n openshift-ingress-operator  -o yaml
```

... ^ and strip out the things you didn't need and save that in Git.

## SSA Apply

Now you just save the delta (in [openshift/app/openshift-ingress-patch.yaml](openshift/app/openshift-ingress-patch.yaml)), example:

```yaml
apiVersion: operator.openshift.io/v1
kind: IngressController
metadata:
  annotations:
    argocd.argoproj.io/sync-options: Validate=false,ServerSideApply=true
  name: default
  namespace: openshift-ingress-operator
spec:
  replicas: 3
```

Note the annotations. Read more about SSA [here](https://argo-cd.readthedocs.io/en/stable/user-guide/sync-options/#server-side-apply)

Apply the [Argo CD Application](openshift/ssa-app-example.yaml)

```shell
oc apply -n openshift-gitops -f https://raw.githubusercontent.com/christianh814/ssa-test/main/openshift/ssa-app-example.yaml
```
