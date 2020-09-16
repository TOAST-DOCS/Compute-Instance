## Compute > Instance > Troubleshooting Guide 

The document describes how to resolve issues you may encouter while using TOAST. 

<h3> I want to use a different version, other than the default OS version of TOAST. Can I upload my personal images? </h3>

You can only use the OS version provided by TOAST. And uploading personal images is not allowed. 
To use personal OS images, create an instance with TOAST image and apply **Create Image**.  
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

<h3>개인 I find error mounting, after creating image and instance, and booting it. </h3>

You shall encounter with such error, when an image is created with instance using two or more block storages and instance is created and booted with such image. 

For instances that use more than two block storages, set disks other than default disk in the `/etc/fstab` file. Since the file is to be replicated as well, when image is created, error occurs in mounting
due to lack of a block storage referenced by the`/etc/fstab` file.  


To resolve this issue, set block storage of the `/etc/fstab` file, except default disk, as footnote, before creating an image. 
<br>

<h3> It takes too long to access SSH. </h3>

It happens when DNS is blocked at the receiving part of the security group to which instance belongs. Adjust the security group to be allowed to receive DNS. 
<br>

<h3> I find "Could not resolve the host" and cannot use yum.  메시지가 나타나며 yum 등을 사용할 수 없습니다.</h3>

It happens when DNS is blocked at the receiving part of the security group to which instance belongs. Adjust the security group to be allowed to receivie DNS. 
<br>
