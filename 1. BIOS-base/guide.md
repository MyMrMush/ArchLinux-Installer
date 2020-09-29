# Guide for installing on BIOS system
(We are using sdx representative for the disk you wish to install on)


use 'cfdisk' to create following partitioning:

    - sdx1  512M    83, Bootable
    - sdx2  Rest    83


Load dm-crypt module:

    modprobe dm-crypt


Encrypt your root partition:

    cryptsetup -c aes-xts-plain -y -s 512 luksFormat /dev/sdx2
    cryptsetup luksOpen /dev/sdx2 lvm


create the LVM:

    pvcreate /dev/mapper/lvm
    vgcreate main /dev/mapper/lvm


create logical volumes: (you may change the size of the swap according to your needs/preferences)

    lvcreate -L 4G main -n swap
    lvcreate -l 100%FREE main -n root


create swap:

    mkswap /dev/mapper/main-swap
    swapon /dev/mapper/main-swap


create boot partition:

    mkfs.ext4 /dev/sda1


create the btrfs filesystem and subvolumes, mount everything:

    mkfs.btrfs -m single -L arch /dev/mapper/main-root
    mount -o compress=lzo /dev/mapper/main-root /mnt
    cd /mnt
    btrfs su cr @
    btrfs su cr @home
    btrfs su cr @log
    btrfs su cr @pkg
    btrfs su cr @srv
    btrfs su cr @tmp
    cd /
    umount /mnt
    mount -o compress=lzo,subvol=@ /dev/mapper/main-root /mnt
    cd /mnt
    mkdir -p {boot,home,srv,var/{log,cache/pacman/pkg,tmp}}
    mount /dev/sdx1 /mnt/boot
    mount -o compress=lzo,subvol=@home /dev/mapper/main-root home
    mount -o compress=lzo,subvol=@log /dev/mapper/main-root var/log
    mount -o compress=lzo,subvol=@pkg /dev/mapper/main-root var/cache/pacman/pkg
    mount -o compress=lzo,subvol=@srv /dev/mapper/main-root srv
    mount -o compress=lzo,subvol=@tmp /dev/mapper/main-root var/tmp


install the base system:

    cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
    grep -E -A 1 ".*Germany.*$" /etc/pacman.d/mirrorlist.bak | sed '/--/d' > /etc/pacman.d/mirrorlist

    pacstrap /mnt base base-devel linux linux-firmware vim bash-completion fish
    pacstrap /mnt wpa_supplicant netctl dialog networkmanager cronie cups avahi dbus acpid dhcpcd git 
    pacstrap /mnt xf86-input-synaptics lvm2 snapper


based on Processor either

    pacstrap /mnt intel-ucode

or

    pacstrap /mnt amd-ucode


generate the fstab:

    genfstab -Up /mnt > /mnt/etc/fstab
    cat /mnt/etc/fstab


continue in chroot.md from here :)