# Configuring and deploying the dataplane

## Assumptions

- The [control plane](control-plane.md) has been created and successfully deployed

## Initialize

Switch to the "openstack" namespace
```
oc project openstack
```
Change to the nova/nova01alpha/edpm directory
```
cd architecture/examples/dt/nova/nova01alpha/edpm/
```
Edit the [nodeset/values.yaml](edpm/nodeset/values.yaml) and [deployment/values.yaml](edpm/deployment/values.yaml) files to suit 
your environment.
```
vi nodeset/values.yaml
vi deployment/values.yaml
```
Generate the dataplane nodeset CR.
```
kustomize build nodeset > dataplane-nodeset.yaml
```
Generate the dataplane deployment CR.
```
kustomize build deployment > dataplane-deployment.yaml
```

## Create CRs
Create the nodeset CR
```
oc apply -f dataplane-nodeset.yaml
```
Wait for dataplane nodeset setup to finish
```
oc wait osdpns openstack-edpm --for condition=SetupReady --timeout=600s
```

Start the deployment
```
oc apply -f dataplane-deployment.yaml
```

Wait for dataplane deployment to finish
```
oc wait osdpns openstack-edpm --for condition=Ready --timeout=40m
```

## Finalize necessary GPU Driver installation
Change to the nova/nova01alpha/edpm-post-driver directory
```
cd architecture/examples/dt/nova/nova01alpha/edpm-post-driver/
```
Edit the [deployment/values.yaml](edpm-post-driver/deployment/values.yaml) files to suit 
your environment.
```
vi deployment/values.yaml
```
Generate the dataplane deployment CR.
```
kustomize build deployment > dataplane-post-driver-deployment.yaml
```

## Create CRs
Start the deployment
```
oc apply -f dataplane-post-driver-deployment.yaml
```

Wait for dataplane deployment to finish
```
oc wait osdpd edpm-deployment-post-driver --for condition=Ready --timeout=20m
```