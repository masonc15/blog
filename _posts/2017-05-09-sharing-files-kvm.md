---
layout: post
title: Sharing files between host and guests in qemu/kvm
---

Lately I have been playing around with qemu/kvm. Which has been fun. But I have had some trouble getting file-sharing between host and guest to work properly. A few of the tips and tutorials were quite old, and others just didn't work for me. So this is how I got it to work in the end. I used virt-manager, but it can be done in virsh as well.

 
1. Create a folder that you would like to share between host and guest. So this folder I am creating in the host: `/home/xapax/share`.
2. Now we need to change the owner of the folder, it has to be owned by libvirt-qemu, otherwise you will get an error when you try to start the VM, and it just won't start. There might be a better way to set up the permissions but this is how I did it: `chown libvirt-qemu /home/xapax/share`
3. In virt-manager, or virsh, go to the machine information and click "Add hardware", then to "Add filesystem".

```
Type: Mount
Mode: Squash (Don't use passthrough)
Source-path: /home/xapax/share
Target-path: hostshare (this is just a word that we use to refer to this filesystem - so it is not really a "path")
```

4. Start the VM. Open a terminal.
5. Create the directory where you will mount the filesystem: `mkdir /home/guest_user/host_files`
6. Now let's mount the filesystem. `mount -t 9p -o trans=virtio,version=9p2000.L /hostshare /home/guest_user/host_files`
7. The share can not be mounted and edited on several hosts at the same time. So make sure to umount it before mounting it to another guest host.

It is that simple.


