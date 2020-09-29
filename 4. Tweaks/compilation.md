# Compilation configuration

without any additional configuration makepkg will ony use one core to compile and compress packages.
We can change this by adding the desired makeflags. The rule with this is the count of the (virtual) cores +1.

For example for an i9-9900k which is an 8/16 core CPU:

    change /etc/makepkg.conf
    MAKEFLAGS="-j17"
    COMPRESSXZ=(xz -c -z - --threads=0)

