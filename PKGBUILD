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
        "git+https://megous.com/git/p-boot"
        "boot.conf")

sha256sums=('0d37851a3f6e8cc35c00e771c9147ba854d3cc3f06ba2bda07427bb849872c23'
            'SKIP'
            'SKIP')

# Get boot partition name from mounted root, assume partition 1 is boot
_boot=$(mount | grep 'on / ' | awk '{ print $1 }' | sed 's/.$/1/')


check() {
    echo "Boot: ${_boot}"
}

package() {
    # Install files needed for p-boot
    cd "${srcdir}"
    install -Dm644 boot.conf "${pkgdir}/p-boot/boot.conf"

    cd "${srcdir}/pp-${pkgver}"
    install -Dm644 Image "${pkgdir}/p-boot/Image"
    install -Dm644 board-1.2.dtb "${pkgdir}/p-boot/board-1.2.dtb"
    
    cd "${srcdir}/p-boot/dist"
    install -Dm644 fw.bin "${pkgdir}/p-boot/fw.bin"
    install -Dm644 p-boot.bin "${pkgdir}/p-boot/p-boot.bin"
    install -Dm644 p-boot-conf "${pkgdir}/p-boot/p-boot-conf"

    cd "${srcdir}/p-boot/example/files"
    install -Dm644 off.argb "${pkgdir}/p-boot/files/off.argb"
    install -Dm644 pboot2.argb "${pkgdir}/p-boot/files/pboot2.argb"

    # Install modules without symlinks
    _kernver=$(ls ${srcdir}/pp-${pkgver}/modules/lib/modules)
    cd "${srcdir}/pp-${pkgver}/modules/lib/modules/${_kernver}"
    for file in *; do
        if [[ -L $file ]]; then rm $file; fi
    done
    mkdir -p ${pkgdir}/lib/modules/${_kernver}
    cp -R * ${pkgdir}/lib/modules/${_kernver}

    # Generate new initramfs

}
