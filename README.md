# ops/packer

Create VM templates for usage with libvirt/KVM virtualization

# Pre-requisites

 * libvirt/KVM
 * Packer (in /opt/packer)
 * jq


# Build

```
$ cd centos7
$ make
```

# Deploy

For usage with virt-builder, an entry in the index.asc file needs to be created:

```
[idrc-centos-73]
name=CentOS 7.3 IDRC
arch=x86_64
file=template-centos73-x86_64.qcow2.xz
checksum[sha512]=5ed11374668882c05cc0eff873dfeb00f74669994bbef9160957b8161d4f4cb1ca6622dfb5b380b55711994efe9988a638f59e2e3d46bb5eaed378fc05eb1d23
format=qcow2
size=10737418240
compressed_size=250340872
expand=/dev/vda1
```

 * Size from `qemu-img info file.qcow2` (virtual size, in bytes)

 * Compressed size from `ls -l file.qcow2` (in bytes)

 * The checksum should be obtained from the compressed qcow2.xz image.

Once the /var/lib/libvirt/templates/index.asc file has been updated on each KVM host, run `virt-builder --delete-cache` before deploying any VMs.

The final qcow2.xz file should be copied over to /var/lib/libvirt/templates.
