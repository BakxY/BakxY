# Shrink QCOW2 Drive in Proxmox

This guide is targeted at a Proxmox system with a Debian VM. The boot disk of the Debian VM should be shrinked to a smaller size.

1. Create a backup of the target VM
2. Boot the target VM into gparted and open a terminal
3. Switch to root user `sudo su` and `cd`
4. `e2fsck -f /dev/mapper/<filesystem>`
5. `resize2fs /dev/mapper/<filesystem> <size>`
6. `lvreduce -L <size> /dev/mapper/<filesystem>`
7. RESIZING DRIVE IN PROXMOX IN THE WORKS
