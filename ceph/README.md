# CEPH Volume Provisioner for Kubernetes 1.5+

`k8s-rbd-provisioner` and `k8s-cephfs-provisioner` is an out-of-tree dynamic provisioner for Kubernetes 1.5+.
You can use it quickly & easily deploy ceph RBD and CEPHFS storage that works almost anywhere.

It works just like in-tree dynamic provisioner. For more information on how dynamic provisioning works, 
see [the docs](http://kubernetes.io/docs/user-guide/persistent-volumes/) or [this blog post](http://blog.kubernetes.io/2016/10/dynamic-provisioning-and-storage-in-kubernetes.html).

## Development
Compile the provisioners

```make```

Make the container images and push to the registry

```make push```

## Install

See [DEPLOY_CEPHFS.md](https://github.com/frederic-blanc/k8s-provisioner/blob/master/ceph/DEPLOY_CEPHFS.md) for cephfs installation, and see [DEPLOY_RBD.md](https://github.com/frederic-blanc/k8s-provisioner/blob/master/ceph/DEPLOY_RBD.md) for rbd 

## Known limitations

* Kernel CephFS doesn't work with SELinux, setting SELinux label in Pod's securityContext will not work.
* Kernel CephFS doesn't support quota or capacity, capacity requested by PVC is not enforced or validated.
* Currently each Ceph user created by the provisioner has `allow r` MDS cap to permit CephFS mount.

## Acknowledgements

- This provisioner is extracted from [Kubernetes core](https://github.com/kubernetes/kubernetes) with some modifications for this project.
