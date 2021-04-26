# pinebook-pro-fedora-installer
* [General info](#general-info)
* [Dependencies](#dependencies)
* [Usage](#usage)
* [Known Issues](#known-issues)
* [Things to do](#things-to-do)
* [Supported Devices](#supported-devices)
* [Supported Editions](#supported-editions)
* [To create image](#to-create-image)
* [Prebuild images](#prebuild-images)
* [Other notes](#other-notes)
* [Credits](#credits)

## General info
Scripts for installing Fedora aarch64 directly to SD/eMMC. The script will add copr repository for some extra packages needed on Pinebook Pro.  
copr: https://copr.fedorainfracloud.org/coprs/aptupdate/pinebook-pro/  
source: https://github.com/bengtfredh/pinebook-pro-copr.git  

Script will install kernel-pbp from copr which is a vanilla kernel patched for Pinebook Pro with Manjaro config merged on Fedora config. You can chose to switch to linux-manjaro from same copr which is a rpm packaged Manjaro aarch64 kernel. Biggest difference is, kernel-pbp have SELINUX and btrfs build in kernel, linux-manjaro have no SELINUX and have btrfs as module.

This script is "interactive". Meaning that it asks you questions when run to customize your install. Like username, password etc.

Runtime is approx 40-50 minutes for Fedora Workstation depending on your bandwidth and SD card speed. Fedora update is what takes longest time.

## Dependencies:
* systemd-container (systemd-nspawn)
* bash
* wget
* dialog
* libarchive
* qemu-user-static (archlinux: binfmt-qemu-static)
* openssl
* gawk
* polkit

## Usage:
To use this script, please make sure that the following is correct:
* an **empty** SD/eMMC card with at least 16 GB storage is plugged in, but not mounted.
* that your user account has `sudo` rights.

Then use this to get it:
```
git clone https://github.com/bengtfredh/pinebook-pro-fedora-installer
cd pinebook-pro-fedora-installer
chmod +x fedora-installer
sudo bash ./fedora-installer [<url>]
```
Some Fedora mirrors are veeeery slow - give url to known good mirror as parameter i.e.
```
bash ./fedora-installer https://fedora.uib.no/fedora/linux/releases/33/Workstation/aarch64/images/Fedora-Workstation-33-1.3.aarch64.raw.xz
```
## Known Issues:
* Because `dialog` is weird, the script needs to be run in `bash`.
* First boot can take som time for SELINUX autorelabel to run.
* systemd-nspawn --resolv-conf has been buggy. If you have issues you can try,  
  --resolv-conf=auto --resolv-conf=copy-host or --bind-ro=/etc/resolv.conf  
  It all depends how resolv.conf is managed on host and in image.  

## Things to do:
* [x] ~~Use Fedora kernel - default kernel will not boot - maybe build custom.~~ https://github.com/bengtfredh/pinebook-pro-copr/kernel-pbp/
* [x] ~~Get sound to work better, can only get low volume - change setting in overlay~~
* [x] ~~Add support for update-uboot - need overlays for script~~
* [x] ~~Test update-uboot~~
* [x] ~~Create kernel upgrade script~~
* [ ] Add u-boot gfx - I have been testing this and it is too buggy by now. I like https://github.com/pcm720/u-boot-build-scripts/releases.
* [x] ~~Change disklayout and filesystem to btrfs.~~
* [x] ~~Create copr repo and replace overlay and Manjaro part of the script~~ https://github.com/bengtfredh/pinebook-pro-copr

## Supported Devices:
* Pinebook Pro

## Supported Editions:
* Fedora Workstation (default)
* Fedora Minimal (Add full url as parameter to script)
Example:
```
bash ./fedora-installer https://mirrors.dotsrc.org/fedora-buffet/fedora-secondary/releases/33/Spins/aarch64/images/Fedora-Minimal-33-1.2.aarch64.raw.xz
```

There is no reason why this script should not work with other editions of Fedora. Just give url as maramater when script run. Script is tested with Fedora Workstation and Minimal.

## To create image:
To create image you later can dd to mmc/emmc/nvme  
Prepare an image by running following:
```
fallocate -l 8GiB myimage.img
losetup /dev/loopX myimage.img
```
Run the script pointing to /dev/loopX.
* You need to flash image with a tool that preserve UUID like dd.
* You should expand and grow root partition/filesystem while offline.

## Prebuild images
I think the installer is better choice, because you are in control of what you install. But I have created some prebuild images for you. There is images with root on ext4 and on btrfs, see filename:  
https://s3.fredhs.net/minio/pinebook-pro-image/

## Other notes:

This script **should** be distro-agnostic, which means you can install *pinebook-pro-fedora-installer* from **any** distro, as long as the dependencies are met.
  
## Credits
Inspiration from:  
https://gitlab.manjaro.org/manjaro-arm/applications/manjaro-arm-installer  
https://github.com/nikhiljha/pp-fedora-sdsetup  
https://github.com/daniel-thompson/pinebook-pro-debian-installer.git  
