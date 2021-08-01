# PinePhone and Arch Linux ARM

- p-boot bootloader by Megous
- optimized PinePhone kernel from Megous
- needs stock Arch Linux ARM and some setup to start with:
  - https://xnux.eu/howtos/install-arch-linux-arm.html

All bootloader related files are installed to /p-boot -directory.  
**Use with care** - The PKGBUILD comes with pacman hooks that overwrite your boot partition!

## How to use:
On your Pinephone with Arch Linux ARM:
- install git, base-devel and f2fs-tools
- git clone this repo
- cd to repo and run makepkg
- use pacman -U to install the package

## References:
- https://xnux.eu/index.html
- https://megous.com/git/p-boot
- https://xff.cz/kernels
- https://archlinuxarm.org/platforms/armv8/allwinner/pine64

## Output from installation
```
[alarm@alarm pinephone-arch]$ sudo pacman -U linux-megous-5.14-1-aarch64.pkg.tar.xz
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
(1/5) Generate initramfs for p-boot
==> Starting build: 5.14.0-rc2-00324-g7d8bfadc894b
  -> Running build hook: [base]
  -> Running build hook: [udev]
  -> Running build hook: [autodetect]
  -> Running build hook: [modconf]
  -> Running build hook: [block]
  -> Running build hook: [filesystems]
  -> Running build hook: [keyboard]
  -> Running build hook: [fsck]
==> Generating module dependencies
==> Creating gzip-compressed initcpio image: /p-boot/initramfs.img
==> Image generation successful
(2/5) Update boot partition with p-boot required filesystem
Data space:

    00010800-0001a3b2: /p-boot/board-1.2.dtb (size 38 KiB)
    0001a400-00028b64: /p-boot/fw.bin (size 57 KiB)
Kernel Image detected: text_offset=0x00000000
    00028c00-00e71c08: /p-boot/Image (size 14628 KiB)
    00e71e00-01574fb7: /p-boot/initramfs.img (size 7180 KiB)
    01575000-01969800: /p-boot/files/off.argb (size 4050 KiB)
    01969800-01d5e000: /p-boot/files/pboot2.argb (size 4050 KiB)

Boot configurations:

no=0 (pp-5.14.0-rc2)

  console=ttyS0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=f2fs rw rootwait

  D 00010800-0001a3b2 /p-boot/board-1.2.dtb
  A 0001a400-00028b64 /p-boot/fw.bin
  L 00028c00-00e71c08 /p-boot/Image
  I 00e71e00-01574fb7 /p-boot/initramfs.img

file list block 1

  01575000-01969800 off.argb /p-boot/files/off.argb
  01969800-01d5e000 pboot2.argb /p-boot/files/pboot2.argb

Total filesystem size 30072 KiB

(3/5) Put p-boot.bin into the start of the boot device
32+0 records in
32+0 records out
32768 bytes (33 kB, 32 KiB) copied, 0.000966947 s, 33.9 MB/s
(4/5) Arming ConditionNeedsUpdate...
(5/5) Updating module dependencies...
```
