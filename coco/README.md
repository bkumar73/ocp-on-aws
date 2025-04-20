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

* Create KataConfig
```
oc create -f kata-config.yaml
```

* Install NFD Operator if needed.
Install From OpenShift GUI by following the default steps to install an Operator.

Or Install from CLI.
* Create a Namespace, OperatorGroup an Subscription.

```
oc create -f nfd/ns.yaml
oc create -f nfd/og.yaml
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

* Create Runtime Class
```
oc create -f runtime-class.yaml
```

* Set Agent Kernel Parameters. Export Trustee URL variable
```
export trustee_url="Trustee URL"
```

* Set Kernel Paratmer variable
```
export kernel_params="agent.aa_kbc_params=cc_kbc::$trustee_url"
```
* Set the base64 into a variable called source
```
export source=`echo "[hypervisor.qemu]
kernel_params=\"$kernel_params\"" | base64 -w0`
```
* Create MC for Kernel config
```
oc create -f set-kernel-parameter-kata-agent.yaml
```
