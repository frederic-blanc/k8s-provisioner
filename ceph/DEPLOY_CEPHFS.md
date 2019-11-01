# Deployment

## Install instruction

* Start Kubernetes local cluster

See https://kubernetes.io/.

* Create a Ceph admin secret

```bash
ceph auth get-key client.admin > /tmp/secret
kubectl create ns cephfs
kubectl create secret generic ceph-secret-admin --from-file=/tmp/secret --namespace=cephfs
```

* Install with RBAC roles

```
sed -i "s/%%ARCH%%/$(uname -m)/g" deploy/cephfs/deployment.yaml
kubectl apply -f deploy/cephfs
```

* Create a CephFS Storage Class

Replace Ceph monitor's IP in [examples/cephfs/class.yaml](class.yaml) with your own and create storage class:

```bash
kubectl create -f examples/cephfs/class.yaml
```

* Create a claim

```bash
kubectl create -f examples/cephfs/claim.yaml
```

* Create a Pod using the claim

```bash
kubectl create -f examples/cephfs/test-pod.yaml
```