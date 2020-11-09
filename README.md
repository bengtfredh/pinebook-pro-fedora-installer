# pinebook-pro-fedora-installer

Scripts for installing Fedora aarch64 directly to SD/eMMC. You will get Manjaro kernel, rest is pure Fedora.

This script is "interactive". Meaning that it asks you questions when run to customize your install. Like username, password etc.

Runtime is approx 40-50 minutes for Fedora 32 Workstation depending on your bandwidth and SD card speed. Fedora update is what takes longest time.

## Dependencies:
* systemd-container (systemd-nspawn)
* bash
* coreutils
* wget
* git
* systemd
* dialog
* parted
* libarchive
* qemu-user-static (archlinux: binfmt-qemu-static)
* openssl
* gawk
* dosfstools
* polkit

## Installing and using from github:
To use this script, please make sure that the following is correct:

* Some Fedora mirrors are veeeery slow - give url to known good mirror as parameter i.e.
```
bash ./fedora-installer https://fedora.uib.no/fedora/linux/releases/32/Workstation/aarch64/images/Fedora-Workstation-32-1.6.aarch64.raw.xz
```
* an **empty** SD/eMMC card with at least 16 GB storage is plugged in, but not mounted.
* that your user account has `sudo` rights.

Then use this to get it:
```
git clone https://github.com/bengtfredh/pinebook-pro-fedora-installer
cd pinebook-pro-fedora-installer
chmod +x fedora-installer
sudo bash ./fedora-installer [<url>]
```

## Known Issues:
* Because `dialog` is weird, the script needs to be run in `bash`.
* Reboot make Pinebook to hang when start up. Use poweroff and turn on.

## Things to do/improve

- [ ] Use Fedora kernel - default kernel will not boot - maybe build custom.
- [X] ~~Get sound to work better, can only get low volume - change setting in overlay~~
- [X] ~~Change disk layout - I guess @daniel-thompson have a more sane layout~~
- [X] ~~Add support for update-uboot - need overlays for script~~
- [X] ~~Test update-uboot~~
- [X] ~~Create kernel upgrade script~~
- [ ] Add u-boot gfx - I have been testing this and it is too buggy by now. I like https://github.com/pcm720/u-boot-build-scripts/releases.
- [ ] Change disklayout and filesystem to btrfs.
- [ ] Change source to smaller image (container.tar.zx) and dnf an installation.
- [X] ~~Create copr repo and replace overlay and Manjaro part of the script~~ https://github.com/bengtfredh/pinebook-pro-copr

## Usage
Command for update uboot from Fedora:
```
update-uboot --target=pinebook-pro-rk3399 --media=/dev/mmcblkX
```

## Upgrade kernel
Script /usr/bin/kernel-upgrade<BR>
Become root and run `kernel-upgrade` . Script will upgrade kernel on local installation.

## Supported Devices:
* Pinebook Pro

## Supported Editions / Desktops:
* Fedora Workstation (default)
* Fedora Minimal (Add full url as parameter to script)
Example:
```
bash ./fedora-installer https://mirrors.dotsrc.org/fedora-buffet/fedora-secondary/releases/32/Spins/aarch64/images/Fedora-Minimal-32-1.6.aarch64.raw.xz
```

There is no reason why this script should not work with other editions of Fedora. Just give url as maramater when script run. Script is tested with Fedora Workstation and Minimal.

## To create image you later can dd to mmc/emmc/nvme
Prepare an image by running following:
```
fallocate -l 10GiB myimage.img
losetup /dev/loopX myimage.img
```
Run the script pointing to /dev/loopX.
* You need to flash image with a tool that preserve UUID like dd.
* You should expand and grow root partition/filesystem.

## Other notes:

This script **should** be distro-agnostic, which means you can install *pinebook-pro-fedora-installer* from **any** distro, as long as the dependencies are met.
  
## Credits
Inspiration and code parts from:<BR>
https://gitlab.manjaro.org/manjaro-arm/applications/manjaro-arm-installer<BR>
https://github.com/nikhiljha/pp-fedora-sdsetup<BR>
https://github.com/daniel-thompson/pinebook-pro-debian-installer.git
