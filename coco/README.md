# Steps to install CoCo BareMetal SNO

* Install OpenShift SandBox Container Operator.
Install From OpenShift GUI by following the default steps to install an Operator.

Or Install from CLI.
* Create a Namespace, OperatorGorup and Subscription.

```
oc create -f ns.yaml
oc create -f og.yaml
oc create -f subscription.yaml
```
* Create CoCo feature gate ConfigMap
```
oc create -f osc-fg-cm.yaml
```
* Create Layered Image FG ConfigMap. Update the Image location to local mirror for disconnected environment.

```
oc create -f layeredimage-cm-snp.yaml
```

* Install NFD Operator if needed.
Install From OpenShift GUI by following the default steps to install an Operator.

Or Install from CLI.
* Create a Namespace.

```
oc create -f nfd/ns.yaml
```
* Install OperatorGorup

```
oc create -f nfd/og.yaml
```
* Create Subscription 

```
oc create -f nfd/subscription.yaml
```

* Apply NFD CR

```
oc create -f nfd/nfd-cr.yaml
```
* Create AMD SNP-SEV rules

```
oc create -f nfd/amd-rules.yaml 
```
