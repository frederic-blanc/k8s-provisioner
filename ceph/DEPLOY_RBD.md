# Deployment

## Install instruction

* Start Kubernetes local cluster

See https://kubernetes.io/.

* Create a Ceph pool and client key

```bash
ceph    osd pool    delete              kube    kube --yes-i-really-really-mean-it
ceph    osd pool    create              kube    128
ceph    osd pool    application enable  kube    rbd

ceph   auth get-or-create client.kube mon 'allow r' osd 'allow class-read object_prefix rbd_children, allow rwx pool=kube' -o /etc/ceph/ceph.client.kube.keyring
```

* Create a RBD Storage Class

Replace Ceph monitor's IP in [examples/rbd/class.yaml](class.yaml) with your own and create storage class:

>>>>>>> 7df8ffd39d01d7dd72b5f79d259e3e8d9c4ad44d
```bash
cat > deploy/k8s-rbd-storageclass.yaml <<EOF
---
apiVersion: v1
kind: Secret
metadata:
  name: ceph-admin-secret
  namespace: rbd-provisioner
data:
  key: $(ceph auth get-key client.admin | base64)
type: kubernetes.io/rbd
---
apiVersion: v1
kind: Secret
metadata:
  name: ceph-user-secret
  namespace: rbd-provisioner
data:
  key: $(ceph auth get-key client.kube  | base64)
type: kubernetes.io/rbd
---
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: rbd
  namespace: rbd-provisioner
provisioner: ceph.com/rbd
parameters:
  monitors: $(ceph mon dump 2>/dev/null | sed -n 's|^[0-9]\+: \(.*\)/.*$|\1|p' | tr -s '\n' ',')
  pool: kube
  adminId: admin
  adminSecretNamespace: rbd-provisioner
  adminSecretName: ceph-admin-secret
  userId: kube
  userSecretNamespace: rbd-provisioner
  userSecretName: ceph-user-secret
  imageFormat: "2"
  imageFeatures: layering
---
EOF
```

* Install with RBAC roles

```bash
kubectl apply -f deploy/k8s-rbd-provisioner.yaml
kubectl apply -f deploy/k8s-rbd-storageclass.yaml
```

* Create a Pod with the claim

```bash
kubectl create -f examples/test-rbd-pod.yaml
```
