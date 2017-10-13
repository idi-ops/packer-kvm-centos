# packer-centos-internal-use

Create VM templates for usage with libvirt/KVM virtualization

## Rationale

This packer repository differs significantly from [idi-ops/packer-centos](https://github.com/idi-ops/packer-centos), which is to be consumed by developers. At the cost of some code duplication, spagetthi code and unnecessary complexity is avoided by having separate repositories. Additionally, improvements can be made independently without fear of breaking the production-grade CentOS image with changes that only make sense for CentOS Vagrant boxes, and vice-versa.

# Pre-requisites

 * libvirt/KVM
 * Packer (in /opt/packer)
 * jq


## Build

```
$ cd centos7
$ make
```

## Deploy

For usage with virt-builder, an entry in the index.asc file needs to be created:

```
[idrc-centos-74]
name=CentOS 7.4 IDRC
arch=x86_64
file=template-centos74-x86_64.qcow2.xz
checksum[sha512]=5015c2ac947445e9329d2503e1fcb68e1b9d2e39a53740d93f76a701709c26f73900a7e6df9825b32089ecb98fa7f9ded6898e0890651125f55903ab559094a5
format=qcow2
size=10737418240
compressed_size=263800068
expand=/dev/vda1
```

 * Size from `qemu-img info file.qcow2` (virtual size, in bytes)

 * Compressed size from `ls -l file.qcow2` (in bytes)

 * The checksum should be obtained from the compressed qcow2.xz image.

Once the /var/lib/libvirt/templates/index.asc file has been updated on each KVM host, run `virt-builder --delete-cache` before deploying any VMs.

The final qcow2.xz file should be copied over to /var/lib/libvirt/templates.
