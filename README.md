# Kuberentes with Vmware for home lab
Let's learn this shit. 

I will try to colect here all in one place for setup so called bare-metal(mostly aluminium in my setup).

I hope it will grow in some nice tuturial someday:)

# Prerequisites
## System
 My setup:
 Ryzen 7 2800x 64GB 2x 500GB disk
## Software
 ESXi 7
 Vcenter(noob here - I needed little less then 500 GB for this)
## Other
SSH key

# Installation
## Prepare images
Fallow the instructions here:

1. step
https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-set-up-tkg.html
2.step - Prepare to Deploy Management Clusters to vSphere
https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere.html
3. step - Deploy Management Clusters to vSphere with the Installer Interface
https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere-ui.html

Her you need to do in your console(https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-verify-deployment.html):

For management cluster(This needs to be doen first):
```
tkg get management-cluster
kubectl config use-context my-management-cluster-admin@my-management-cluster 

``` 

For cluster(https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-tanzu-k8s-clusters-create.html):
```
tkg create cluster my-cluster --plan dev
```
On the link you can see other options. 

## Prepare LB
https://metallb.universe.tf/
## Prepare Ingress
https://github.com/traefik/traefik/
## Prepare something that  I do not know yet 

# Usage - example

# What I need to figure out:
 - Deploy clusters on different datastore
 - ssh to node/master
 

