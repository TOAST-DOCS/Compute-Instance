## Compute > Instance > Troubleshooting Guide

The document describes how to resolve issues you may encounter while using NHN Cloud.

<h3> I want to use a different version, other than the default OS version of NHN Cloud. Can I upload my personal images? </h3>

You can only use the OS version provided by NHN Cloud. And uploading personal images is not allowed.
To use personal OS images, create an instance with NHN Cloud image and apply **Create Image**.
<br>

<h3> When I try to access instance, I find "Permissions 0644 for '/Users/username/.ssh/your-key.pem' are too open." and access is not available. </h3>

It happens when the personal key (PEM key) applied to access instance has invalid authority.
Adjust the authority of personal key file like below.

    $ chmod 600 your-key.pem

<br>

<h3> How do I get the root authority from CentOS instance?  </h3>

To get root authority from CentOS instance, use the 'sudo' command like follows.

    $ sudo su

<br>

<h3> I find error mounting, after creating image and instance, and booting it. </h3>

You shall encounter with such error, when an image is created with instance using two or more block storages and instance is created and booted with such image.

For instances that use more than two block storages, set disks other than default disk in the `/etc/fstab` file. Since the file is to be replicated as well, when image is created, error occurs in mounting due to lack of a block storage referenced by the`/etc/fstab` file.

To resolve this issue, set block storage of the `/etc/fstab` file, except default disk, as footnote, before creating an image.
<br>

<h3> It takes too long to access SSH. </h3>

It happens when DNS is blocked at the receiving part of the security group to which instance belongs. Adjust the security group to be allowed to receive DNS.
<br>

<h3> I find "Could not resolve the host" and cannot use yum. </h3>

It happens when DNS is blocked at the receiving part of the security group to which instance belongs. Adjust the security group to be allowed to receive DNS.
<br>

<h3> Package update fails on CentOS 6.x instance. </h3>

Use the `yum repository` file after modifying the file as follows.
Additional updates are not supported for OS for which official support has ended, so it is recommended that you use a higher version of the OS.
```
$ sudo vi /etc/yum.repos.d/CentOS-Base.repo
...
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=https://vault.centos.org/6.10/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=https://vault.centos.org/6.10/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=https://vault.centos.org/6.10/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
...

$ sudo yum clean all
$ sudo yum repolist
```
