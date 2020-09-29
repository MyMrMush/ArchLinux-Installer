# Guide for Chrooting into the new system and configuring everything

Changeroot into the new system:

    arch-chroot /mnt/


Set the desired hostname for your system:

    echo NEWHOSTNAME > /etc/hostname


Set the desired locale

    echo LANG=en_US.UTF-8 > /etc/locale.conf


Uncomment all wanted locales from '/etc/locale.gen'

Afterwards generate them:

    locale-gen


Settings for vconsole:

    echo KEYMAP=de-latin1 > /etc/vconsole.conf
    echo FONT=lat9w-16 >> /etc/vconsole.conf

Timezone:

    ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime


Configure Pacman '/etc/pacman.conf':

    add 'ILoveCandy'
    uncomment 'Color'
    uncomment the '[multilib]' library
    pacman -Sy


configure sudo:

    EDITOR=vim visudo
    and decomment %wheel* for sudo rights with password


Configure Users & passwords:

    passwd
    useradd -m -g users -s /bin/fish username
    gpasswd -a username wheel
    passwd username


Add encrypt and lvm hooks /etc/mkinitcpio.conf:

    add encrypt lvm2 after block to the HOOKS


Create the mkininitcpio
    
    mkinitcpio -p linux


enable some useful services

    systemctl enable fstrim.timer
    systemctl enable acpid
    systemctl enable avahi-daemon
    systemctl enable org.cups.cupsd
    systemctl enable cronie
    systemctl enable NetworkManager
    systemctl enable --now systemd-timesyncd.service


Install Grub bootloader:

UEFI

    pacman -S grub os-prober efibootmgr gptfdisk dosfstools
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch_linux --recheck

Legacy/BIOS

    pacman -S grub os-prober dosfstools
    grub-install --target=i386-pc /dev/sda


Configure Grub:

    edit /etc/default/grub
    add cryptdevice=/dev/sda2:lvm to GRUB_CMDLINE_LINUX=""
    grub-mkconfig -o /boot/grub/grub.cfg

Now you can reboot into your system (exit; reboot) and/or continue with GUI installations.