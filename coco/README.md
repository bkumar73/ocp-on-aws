# Steps to install CoCo BareMetal SNO

* Install OpenShift SandBox Container Operator.
Install From OpenShift GUI by following the default steps to install an Operator.

Or Install from CLI.
* Create a Namespace.

```
oc create -f ns.yaml
```
* Install OperatorGorup

```
oc create -f og.yaml
```
