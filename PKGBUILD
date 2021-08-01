# Optimized Linux kernel with p-boot bootloader for Pinephone 1.2 and Arch Linux ARM
# Creator: Ondřej Jirman (Megous)

# Maintainer: Tuomo Kuure <tqre@far.fi>

# NOTE: This PKGBUILD assumes bootloader partition to reside at /dev/mmcblkXp1
# If your partitioning differs, this might render your phone unbootable

pkgname=linux-megous
pkgver=5.14
pkgrel=1
pkgdesc='Linux kernel - optimized for Pinephone'
arch=('aarch64')
license=('GPL')
url='https://megous.com/git'
source=("https://xff.cz/kernels/${pkgver}/pp.tar.gz"
        "boot.conf"
        "https://megous.com/git/p-boot/tree/dist/fw.bin"
        "https://megous.com/git/p-boot/tree/dist/p-boot-conf"
        "https://megous.com/git/p-boot/tree/dist/p-boot.bin"
        "https://megous.com/git/p-boot/tree/example/files/off.argb"
        "https://megous.com/git/p-boot/tree/example/files/pboot2.argb")

sha256sums=('0d37851a3f6e8cc35c00e771c9147ba854d3cc3f06ba2bda07427bb849872c23'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

# Get boot partition name from mounted root, assume partition 1 is boot
_boot=$(mount | grep 'on / ' | awk '{ print $1 }' | sed 's/.$/1/')

check() {
    echo "Boot: ${_boot}"
}

package() {
    echo "Package"
}
