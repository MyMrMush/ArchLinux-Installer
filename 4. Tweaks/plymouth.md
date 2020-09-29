# Installing and configuring Plymouth
Disclaimer: Please also refer to https://wiki.archlinux.org/index.php/Plymouth as some niche configurations may reqire special attention.

Installing plymouth:
    
    yay -Syu plymouth-git


edit the '/etc/mkinitcpio.conf':

    HOOKS=(base udev plymouth plymouth-encrypt ...)


make sure kernel parameters in '/etc/default/grub' include:

    quiet splash loglevel=3 rd.udev.log_priority=3 vt.global_cursor_default=0


When using GNOMEs GDM:

    sudo systemctl disable gdm
    yay -Syu gdm-plymouth
    yay -Rnsc gdm
    sudo systemctl enable gdm-plymouth


When encountering issues or when using more complex themes you might need to load the graphics driver in '/etc/mkinitcpio.conf' first:

    MODULES=(i915 ...) // for intel
    MODULES=(nouveau ...) //for open nvidia
    MODULES=(nvidia ...) //for proprietary nvidia


Setting the theme:

    plymouth-set-default-theme -l // list all available themes
    plymouth-set-default-theme -R <theme> // set teme
