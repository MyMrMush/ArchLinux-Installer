# INSTALLING GNOME #

install x

	pacman -S xorg-server xorg-xinit
	pacman -S xorg-drivers
	pacman -S ttf-dejavu ttf-roboto


install gnome
	
    pacman -S gnome gnome-extra
	systemctl enable --now gdm