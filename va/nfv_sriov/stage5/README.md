# Stage 5

Deploy the data plane

## Steps

1. Create Secrets
```bash
oc apply -f ceph_secret.yaml -f nova_ceph.yaml -f nova_migration_ssh_key.yaml
```
2. Create OpenStackDataPlaneNodeSet
```bash
oc apply -f openstackdataplanenodeset.yaml
```
5. Create OpenStackDataPlaneDeployment and wait for it to finish
```bash
oc apply -f openstackdataplanedeployment.yaml
oc wait osdpd deployment-post-ceph --for condition=Ready --timeout=720s
```
6. Force Nova to discover all compute hosts
```bash
oc rsh nova-cell0-conductor-0 nova-manage cell_v2 discover_hosts --verbose
```
