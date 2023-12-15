# Stage 3

Configure OCP cluster networking for OSP

## Steps

1. Create NNCPs
```bash
oc apply -f ocp_node_0_nncp.yaml
oc wait nncp -l osp/interface=enp6s0 --for condition=available --timeout=300s
```
2. Create NetAttachs
```bash
oc apply -f netattach_ctlplane.yaml -f netattach_internalapi.yaml -f netattach_storage.yaml -f netattach_tenant.yaml
```
3. Create MetalLB resources

```bash
oc apply -f metallb_ipaddresspools.yaml -f metallb_l2advertisement.yaml
```

A NetworkAttachmentDefinition is not required for storage management
network since that network is used by OpenStackDataPlane nodes but not
OpenStack pods hosted on OpenShift in this architecture.
