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
        "boot.conf"
        "fstab"
        "10-pp-initramfs.hook"
        "11-p-boot-update.hook"
        "12-p-boot-binary-update.hook")

sha256sums=('0d37851a3f6e8cc35c00e771c9147ba854d3cc3f06ba2bda07427bb849872c23'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

# Assuming boot partition is on the root device as partition 1, if there is an existing
# p-boot installation, the boot partition is not mounted.
_bootdev=$(mount | grep 'on / ' | awk '{ print $1 }' | sed 's/..$//')
_rootpart=$(mount | grep 'on / ' | awk '{ print $1 }')
_bootpart=$(mount | grep 'on / ' | awk '{ print $1 }' | sed 's/.$/1/')
_fstype=$(mount | grep 'on / ' | awk '{ print $5 }')

check() {
    echo "  -> Detected block device path as ${_bootdev}"
    echo "  -> Detected boot partition as ${_bootpart}"
    echo "  -> Detected root partition as ${_rootpart} with ${_fstype} filesystem"
    echo "  If these are NOT correct, press Ctrl-C to abort. These variables are written to pacman hooks which"
    echo "  are executed upon package install. You'll probably break something if they are not correct."
    echo ""
    read -p "Otherwise, press enter to continue."
}

package() {
    # Install files needed for p-boot
    cd "${srcdir}"
    sed -i "s#ROOTPART#${_rootpart}#" boot.conf
    sed -i "s/FSTYPE/${_fstype}/" boot.conf
    install -Dm644 boot.conf "${pkgdir}/p-boot/boot.conf"

    cd "${srcdir}/pp-${pkgver}"
    install -Dm644 Image "${pkgdir}/p-boot/Image"
    install -Dm644 board-1.2.dtb "${pkgdir}/p-boot/board-1.2.dtb"
    
    cd "${srcdir}/p-boot/dist"
    install -Dm644 fw.bin "${pkgdir}/p-boot/fw.bin"
    install -Dm644 p-boot.bin "${pkgdir}/p-boot/p-boot.bin"
    install -Dm744 p-boot-conf "${pkgdir}/p-boot/p-boot-conf"

    cd "${srcdir}/p-boot/example/files"
    install -Dm644 off.argb "${pkgdir}/p-boot/files/off.argb"
    install -Dm644 pboot2.argb "${pkgdir}/p-boot/files/pboot2.argb"

    # Install modules without symlinks
    _kernver=$(ls ${srcdir}/pp-${pkgver}/modules/lib/modules)
    cd "${srcdir}/pp-${pkgver}/modules/lib/modules/${_kernver}"
    for file in *; do
        if [[ -L $file ]]; then rm $file; fi
    done
    mkdir -p ${pkgdir}/usr/lib/modules/${_kernver}
    cp -R * ${pkgdir}/usr/lib/modules/${_kernver}

    # Install pacman hook with right kernel version for initramfs creation
    cd "${srcdir}"
    sed -i "s/KERNELVERSION/${_kernver}/" 10-pp-initramfs.hook
    install -Dm644 10-pp-initramfs.hook "${pkgdir}/etc/pacman.d/hooks/10-pp-initramfs.hook"

    # Install pacman hook for p-boot bootloader spceial filesystem update
    cd "${srcdir}"
    sed -i "s#BOOTPART#${_bootpart}#" 11-p-boot-update.hook
    install -Dm644 11-p-boot-update.hook "${pkgdir}/etc/pacman.d/hooks/11-p-boot-update.hook"

    # Install pacman hook for p-boot binary update to boot device
    cd "${srcdir}"
    sed -i "s#BOOTDEV#${_bootdev}#" 12-p-boot-binary-update.hook
    install -Dm644 12-p-boot-binary-update.hook "${pkgdir}/etc/pacman.d/hooks/12-p-boot-binary-update.hook"

    # Write a new fstab file and install it
    cd "${srcdir}"
    sed -i "s#ROOTPART#${_rootpart}#" fstab
    sed -i "s/FSTYPE/${_fstype}/" fstab
    install -Dm644 fstab "${pkgdir}/etc/fstab"    
}
