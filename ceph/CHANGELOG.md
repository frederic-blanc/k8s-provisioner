# v0.2.0
- merge the two ceph project to have an uniq vndor folder for the building inside a docker container<br/>
  still keep the rbd-provisioner pkhg with its previous import as I change minimal code inside it (see below).
- remove klog.InitFlags(nil) in the rbd-provisioner as it is not needed anymore.
- move cephfs-provisioner go code ti cmd folder.
- Build provisioner inside multistage container

# v0.1.2
## cephfs provisionner
- Use provisioner name as identity by default instead of random string. See #267. (https://github.com/kubernetes-incubator/external-storage/pull/270)
- Add PROVISIONER_NAME environment variable support (https://github.com/kubernetes-incubator/external-storage/pull/270)

# v0.1.1
## cephfs provisioner
- Fix docker file error "chmod: invalid mode: 'x+o'" (https://github.com/kubernetes-incubator/external-storage/pull/215)
## rbd provisioner
- Use provisioner name as identity by default instead of random string. (https://github.com/kubernetes-incubator/external-storage/pull/267)
- Add PROVISIONER_NAME environment variable support (https://github.com/kubernetes-incubator/external-storage/pull/267)

# v0.1.0
- Initial release