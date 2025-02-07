# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ifeq ($(REGISTRY),)
	REGISTRY        = blancfrederic
endif

ifeq ($(VERSION),)
	VERSION         = latest
endif

IMAGE_BUIDLER       = k8s-ceph-provisioner-builder:latest

IMAGENAME_CEPHS     = k8s-cephfs-provisioner
IMAGENAME_RBD       = k8s-rbd-provisioner

IMAGE_CEPHS         = $(REGISTRY)/$(IMAGENAME_CEPHS):$(VERSION)
IMAGE_RBD           = $(REGISTRY)/$(IMAGENAME_RBD):$(VERSION)

MUTABLE_IMAGE_CEPHS = $(REGISTRY)/$(IMAGENAME_CEPHS):latest
MUTABLE_IMAGE_RBD   = $(REGISTRY)/$(IMAGENAME_RBD):latest

all build:
	docker  build   -t  $(IMAGE_BUIDLER)        -f Dockerfile.gobuilder .
.PHONY: all build

container: build quick-container
.PHONY: container

quick-container:
	docker  build   -t  $(MUTABLE_IMAGE_CEPHS)  -f Dockerfile.cephfs    .
	docker  build   -t  $(MUTABLE_IMAGE_RBD)    -f Dockerfile.rbd       .
	
	docker  tag         $(MUTABLE_IMAGE_CEPHS)  $(IMAGE_CEPHS)
	docker  tag         $(MUTABLE_IMAGE_RBD)    $(IMAGE_RBD)
.PHONY: quick-container

push: container
	docker  push        $(MUTABLE_IMAGE_CEPHS)
	docker  push        $(IMAGE_CEPHS)
	docker  push        $(MUTABLE_IMAGE_RBD)
	docker  push        $(IMAGE_RBD)
.PHONY: push

test:
	go test ./...
.PHONY: test

clean:
.PHONY: clean
