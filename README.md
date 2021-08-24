# PinePhone and Arch Linux ARM

- p-boot bootloader by Megous (currently at 5.14-rc6)
- optimized PinePhone kernel from Megous
- needs stock Arch Linux ARM and some setup to start with:
  - https://xnux.eu/howtos/install-arch-linux-arm.html
- can be used with with danctnix's Arch Linux ARM with Phosh GUI
  - https://github.com/dreemurrs-embedded/Pine64-Arch

TODO: LTE Modem (EG25) needs some tuning with danctnix build, as megous' kernel uses modem power manager driver: https://xnux.eu/devices/feature/modem-pp.html  
TODO: pacman configs to avoid kernel updates form danctnix tree

All bootloader related files are installed to /p-boot -directory, which are read with the pacman hooks upon installation.  

**Use with care** - The PKGBUILD comes with pacman hooks that overwrite your boot partition! Use plain `makepkg` to building the package.

## How to use:
On your Pinephone with Arch Linux ARM:
- install git and base-devel 
- install f2fs-tools (not needed with danctnix as it uses ext4 fs on root partition)
- git clone this repo
- cd to repo and run makepkg
- pacman -U linux-megous-<VERSION>-aarch64.pkg.tar.xz --overwrite /etc/fstab

## References:
- https://xnux.eu/index.html
- https://megous.com/git/p-boot
- https://xff.cz/kernels
- https://archlinuxarm.org/platforms/armv8/allwinner/pine64

## Example output from installation
```
[alarm@alarm pinephone-arch]$ sudo pacman -U linux-megous-5.14-1-aarch64.pkg.tar.xz --overwrite /etc/fstab
loading packages...
resolving dependencies...
looking for conflicting packages...

Packages (1) linux-megous-5.14-1

Total Installed Size:  39.00 MiB

:: Proceed with installation? [Y/n]
(1/1) checking keys in keyring                       [############################] 100%
(1/1) checking package integrity                     [############################] 100%
(1/1) loading package files                          [############################] 100%
(1/1) checking for file conflicts                    [############################] 100%
(1/1) checking available disk space                  [############################] 100%
:: Processing package changes...
(1/1) installing linux-megous                        [############################] 100%
:: Running post-transaction hooks...
(1/7) Generate initramfs for p-boot
==> Starting build: 5.14.0-rc6
  -> Running build hook: [base]
  -> Running build hook: [udev]
  -> Running build hook: [autodetect]
  -> Running build hook: [modconf]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [keyboard]
  -> Running build hook: [fsck]
  -> Running build hook: [bootsplash-danctnix]
==> Generating module dependencies
==> Creating gzip-compressed initcpio image: /p-boot/initramfs.img
==> Image generation successful
(2/7) Set up boot partitions for p-boot
Information: You may need to update /etc/fstab.



(3/7) Update boot partition with p-boot required filesystem
Data space:

    00010800-0001a3b2: /p-boot/board-1.2.dtb (size 38 KiB)
    0001a400-00028b64: /p-boot/fw.bin (size 57 KiB)
Kernel Image detected: text_offset=0x00000000
    00028c00-00e71c08: /p-boot/Image (size 14628 KiB)
    00e71e00-015bf52b: /p-boot/initramfs.img (size 7477 KiB)
    015bf600-019b3e00: /p-boot/files/off.argb (size 4050 KiB)
    019b3e00-01da8600: /p-boot/files/pboot2.argb (size 4050 KiB)

Boot configurations:

no=0 (pp-5.14.0-rc6)

  console=tty1 root=/dev/mmcblk2p2 rootfstype=ext4 rw rootwait

  D 00010800-0001a3b2 /p-boot/board-1.2.dtb
  A 0001a400-00028b64 /p-boot/fw.bin
  L 00028c00-00e71c08 /p-boot/Image
  I 00e71e00-015bf52b /p-boot/initramfs.img

file list block 1

  015bf600-019b3e00 off.argb /p-boot/files/off.argb
  019b3e00-01da8600 pboot2.argb /p-boot/files/pboot2.argb

Total filesystem size 30369 KiB

(4/7) Put p-boot.bin into the start of the boot device
32+0 records in
32+0 records out
32768 bytes (33 kB, 32 KiB) copied, 0.000812667 s, 40.3 MB/s
(5/7) Arming ConditionNeedsUpdate...
(6/7) Updating module dependencies...
(7/7) Refreshing PackageKit...
[alarm@danctnix pinephone-arch]$
