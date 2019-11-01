# Deployment

## Install instruction

* Start Kubernetes local cluster

See https://kubernetes.io/.

* Create a Ceph admin secret

```bash
ceph auth get client.admin 2>&1 |grep "key = " |awk '{print  $3'} |xargs echo -n > /tmp/key
kubectl create secret generic ceph-admin-secret --from-file=/tmp/key --namespace=kube-system --type=kubernetes.io/rbd
```

* Create a Ceph pool and a user secret

```bash
ceph osd pool create kube 8 8
ceph auth add client.kube mon 'allow r' osd 'allow rwx pool=kube'
ceph auth get-key client.kube > /tmp/key
kubectl create secret generic ceph-secret --from-file=/tmp/key --namespace=kube-system --type=kubernetes.io/rbd
```

* Install with RBAC roles

```
kubectl apply -f deploy/rbd
```

* Create a RBD Storage Class

Replace Ceph monitor's IP in [examples/rbd/class.yaml](class.yaml) with your own and create storage class:

```bash
kubectl create -f examples/rbd/class.yaml
```

* Create a claim

```bash
kubectl create -f examples/rbd/claim.yaml
```

* Create a Pod using the claim

```bash
kubectl create -f examples/rbd/test-pod.yaml
