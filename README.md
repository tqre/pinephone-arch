# PinePhone and Arch Linux ARM

- p-boot bootloader by Megous
- optimized PinePhone kernel from Megous
- needs stock Arch Linux ARM and some setup to start with:
  - https://xnux.eu/howtos/install-arch-linux-arm.html

All bootloader related files are installed to /p-boot -directory.  
**Use with care** - The PKGBUILD comes with pacman hooks that overwrite your boot partition!

## How to use:
On your Pinephone with Arch Linux ARM:
- install git and base-devel
- git clone this repo
- cd to repo and run makepkg
- use pacman -U to install the package

## References:
- https://xnux.eu/index.html
- https://megous.com/git/p-boot
- https://xff.cz/kernels
- https://archlinuxarm.org/platforms/armv8/allwinner/pine64

