# Setting up Yay as AUR helper

The Arch User Repository (AUR) extends the otherwise quite limited package base of the official repositories.
The officially supported way of installing an AUR application is to clone and buid it manually.

Example manual installation of gotop:

    git clone https://aur.archlinux.org/gotop.git
    cd gotop
    makepkg -si


As this is quite annoying and also leaves the user to manually check for updates and redoing that process when applying one,
 there are so called AUR helpers like Yay, that integrate the AUR packages into the usual package management.

 Setting up Yay:

    git clone https://aur.archlinux.org/yay.git
    cd yay/
    makepkg -si


Yay acts as a pacman wrapper, which means, that instead of 'sudo pacman ... ' you can now use 'yay ...' to also include AUR packages in all the commands.

Some handy commands are:

    yay -Syu <package> // updates all and install a package
    yay -Rnsc <package> // removes a package and its unused dependencies
    yay -Pw // shows important Archwiki new, that might affect your updates