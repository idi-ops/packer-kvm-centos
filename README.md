# CentOS image for KVM/libvirt (IDRC-internal)

This is a set of configuration files and scripts to provision virtual machine and container images in a deterministic way.

## Rationale

This packer repository differs significantly from [idi-ops/packer-centos](https://github.com/idi-ops/packer-centos), which is to be consumed by developers. At the cost of some code duplication, spagetthi code and unnecessary complexity is avoided by having separate repositories. Additionally, improvements can be made independently without fear of breaking the production-grade CentOS image with changes that only make sense for CentOS Vagrant boxes, and vice-versa.

# Pre-requisites

 * libvirt/KVM
 * VirtualBox
 * Vagrant
 * Packer
 * jq

It might be tricky to have many hypervisors running on the same build machine. Beware.

# How to Build

Vagrant:

```
$ cd centos7
$ make validate
$ make virtualbox
```

libvirt/KVM (to be used with virt-builder):

```
$ cd centos7
$ make kvm
```

The libvirt/KVM template needs to be sysprepped before deploying:

```
$ virt-sysprep --operations 'abrt-data,bash-history,blkid-tab,crash-data,cron-spool,customize,dhcp-client-state,dhcp-server-state,dovecot-data,logfiles,lvm-uuids,machine-id,mail-spool,net-hostname,net-hwaddr,pacct-log,package-manager-cache,pam-data,puppet-data-log,rh-subscription-manager,rhn-systemid,rpm-db,samba-db-log,script,smolt-uuid,ssh-hostkeys,sssd-db-log,tmp-files,udev-persistent-net,utmp,yum-uuid,-ssh-userdir' -a file.qcow2
```
