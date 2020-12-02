---
title: virsh
Description: Notes and links on virsh
first-published: 2014-04-09
---

List all VMs:

        virsh list --all

Automatically boot a VM when the host boots:

        virsh autostart vm.zindilis.net

Disable automatic start:

        virsh autostart --disable vm.zindilis.net

Manually power on a VM:

        virsh start vm.zindilis.net

Gracefully shutdown a VM:

        virsh shutdown vm.zindilis.net

Forcefully shutdown a VM:

        virsh destroy vm.zindilis.net
