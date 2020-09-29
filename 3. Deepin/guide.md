# INSTALLING Deepin

install x

	pacman -S xorg-server xorg-xinit
	pacman -S xorg-drivers
	pacman -S ttf-dejavu ttf-roboto


install deepin
	
    pacman -S deepin deepin-extra gdm
	systemctl enable --now gdm