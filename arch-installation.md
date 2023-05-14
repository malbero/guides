# Road to Arch Linux (from MacOS, without `archinstall`)

## Create a bootable USB

Download the ISO Image and check the file integrity by comparing provided checksums (SHA256) with:

`shasum -a 256 /path/to/file`

Connect the USB Drive to your Mac and identify the Device with:

`diskutil list`

Since USB Devices in MacOS are auto-mounted unmount with:

`diskutil unmountDisk /dev/diskX`

Copy the ISO image file to the device:

`dd if=path/to/archlinux-version-x86_64.iso of=/dev/rdiskX bs=1m`

Boot the target system from USB in UEFI Mode

## Base installation

Set German keyboard layout with:

`loadkeys de-latin1`

Use iwctl to connect to a network:

- Type `iwctl` to open an interactive prompt

Use the following commands to establish a connection:

- `device list` // list WIFI devices
- `station deviceX scan` // scan for networks
- `station deviceX get-networks` // list available networks
- `station deviceX connect SSID` // connect to network

Partition disks:

- type `lsblk` to list available disks
- choose the disk you want to format
- format the disk with the following command:

`cfdisk diskX`

Use the menu to create a boot (128M (for grub) should suffice) and root partition with an appropriate size and set the *bootable* flag for the boot partition

Set the ext4 file system for the created partitions with the following command:

`mkfs.ext4 partitionX`

Mount the created partitions with:

- `mnt rootPartition /mnt`
- `mkdir /mnt/boot`
- `mnt bootPartition /mnt/boot`

Install the base system with:

`pacstrap /mnt base base-devel linux linux-firmware vim`

Generate the fstab file to tell linux what drives to load when booting up:

`genfstab /mnt`

Change root from the bootable usb into the actual arch installation:

`arch-chroot /mnt /bin/bash`

Install additional packages via pacman:

`packman -S networkmanager grub`

Enable NetworkManager after boot:

`systemctl enable NetworkManager`

Install the grub boot loader with:

`grub-install --target=i386-pc diskX`

**Hints**:
- choose the disk, not a specific partition
- the `--target=i386-pc` parameter is essential regardless of which os you are running

Create the grub config file with:

`grub-mkconfig -o /boot/grub/grub.cfg`

Uncomment the locales you want to use in the following file:

`vim /etc/locale.gen`

Generate locales with:

`locale-gen`

Create a locale config file:

`vim /etc/locale.conf` and enter your preferred locale with “LANG=*locale*“

You are done with the base installation

Unmount everything with:

`umount -R /mnt`

And reboot with:

`reboot`

## Detailed Setup:

TBC…
