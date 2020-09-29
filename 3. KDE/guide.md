# INSTALLING KDE #

install x

	pacman -S xorg-server xorg-xinit
	pacman -S xorg-drivers
	pacman -S ttf-dejavu ttf-roboto


install kde plasma
	
    pacman -S plasma plasma-wayland-session kde-applications
	systemctl enable --now sddm