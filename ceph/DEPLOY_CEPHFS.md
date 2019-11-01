# Deployment

## Install instruction

* Start Kubernetes local cluster

See https://kubernetes.io/.

* Create secret and StorageClass kubectl file

```bash
cat > deploy/k8s-cephfs-storageclass.yaml <<EOF
---
apiVersion: v1
kind: Secret
metadata:
  name: ceph-admin-secret
  namespace: cephfs-provisioner
data:
  key: $(ceph auth get-key client.admin | base64)
type: kubernetes.io/rbd
---
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: cephfs
  namespace: cephfs-provisioner
provisioner: ceph.com/cephfs
parameters:
    monitors: $(ceph mon dump 2>/dev/null | sed -n 's|^[0-9]\+: \(.*\)/.*$|\1|p' | tr -s '\n' ',')
    adminId: admin
    adminSecretName: ceph-admin-secret
    adminSecretNamespace: "cephfs-provisioner"
    claimRoot: /pvc-volumes
---
EOF
```

* Install with RBAC roles

<<<<<<< HEAD
=======
```
kubectl apply -f deploy/cephfs
```

* Create a CephFS Storage Class

Replace Ceph monitor's IP in [examples/cephfs/class.yaml](class.yaml) with your own and create storage class:

```bash
kubectl create -f examples/cephfs/class.yaml
```

* Create a claim

>>>>>>> 7df8ffd39d01d7dd72b5f79d259e3e8d9c4ad44d
```bash
kubectl apply -f deploy/k8s-cephfs-provisioner.yaml
kubectl apply -f deploy/k8s-cephfs-storageclass.yaml
```

* Create a Pod with the claim

```bash
<<<<<<< HEAD
kubectl create -f examples/test-cephfs-pod.yaml
```
=======
kubectl create -f examples/cephfs/test-pod.yaml
```
>>>>>>> 7df8ffd39d01d7dd72b5f79d259e3e8d9c4ad44d
