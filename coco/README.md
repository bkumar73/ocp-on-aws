# Install Disconnected SNO

* Set up mirror registry for disconnected SNO. An example ImageSetConfiguration can be found at imageset-config.yaml

* Develop install-config.yaml using the example given. Update baseDomain, machineNetwork, installationDisk, pullSecret, imageContentSource, sshKey and additionalTrustBundle as needed.

* Develop agent-config.yaml using the example given. Update rendezvousIP, interface name, macAddress and other details appropriately.

* Extract openshift-install from release image on mirror registry, eg

```
oc adm release extract -a pull-secret.txt --command=openshift-install mirror.hub.mylab.com:8443/openshift/release-images:4.18.5-x86_64
```

* Copy the install-config.yaml and agent-config.yaml to a directory eg "ocp" and create the ISO

```
./openshift-install --dir ocp/ agent create image
```

* Boot the SNO node using the ISO to install OCP.

# Install and Configure Trustee Operator

* Install OpenShift Trustee.
Install From OpenShift GUI by following the default steps to install an Operator.

# Install OSC and Configure CoCo

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

* Install NFD Operator.
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
