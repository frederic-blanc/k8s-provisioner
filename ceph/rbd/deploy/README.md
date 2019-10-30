# Deployment

## Table of contents

* [Install without RBAC roles](#install-without-rbac-roles)
* [Install with RBAC roles](#install-with-rbac-roles)
 
As I have not uploaded the built container anywhere, you must preload it on every nodes of
your k8s cluster

## Install without RBAC roles

```
cd  k8s-ceph-provisioner/ceph/rbd/deploy
sed -i "s/%%ARCH%%/$(uname -m)/g" non-rbac/deployment.yaml
kubectl apply -f ./non-rbac
```

## Install with RBAC roles

```
cd  k8s-ceph-provisioner/ceph/rbd/deploy
sed -i "s/%%ARCH%%/$(uname -m)/g" rbac/deployment.yaml
kubectl apply -f ./rbac
```
