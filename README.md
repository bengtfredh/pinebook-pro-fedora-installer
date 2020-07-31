# pinebook-pro-fedora-installer

Inspiration and code parts from:<BR>
https://gitlab.manjaro.org/manjaro-arm/applications/manjaro-arm-installer<BR>
https://github.com/nikhiljha/pp-fedora-sdsetup<BR>
https://github.com/daniel-thompson/pinebook-pro-debian-installer.git

Scripts for installing Fedora aarch64 directly to SD/eMMC. You will get Manjaro kernel, rest is pure Fedora.

This script is "interactive". Meaning that it asks you questions when run to customize your install. Like username, password etc.

Runtime is approx 40-50 minutes for Fedora 32 Workstation depending on your bandwidth and SD card speed. Fedora update is what takes longest time.

## Dependencies:
* systemd-container (systemd-nspawn)
* bash
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

## Installing and using from gitlab:
To use this script, please make sure that the following is correct:

* Some Fedora mirrors are veeeery slow - change FEDORAURL to a known good mirror.
* an **empty** SD/eMMC card with at least 16 GB storage is plugged in, but not mounted.
* Nothing is mounted on /dev/loop0
* that your user account has `sudo` rights.

Then use this to get it:
```
git clone https://github.com/bengtfredh/pinebook-pro-fedora-installer
cd pinebook-pro-fedora-installer
chmod +x fedora-installer
sudo bash ./fedora-installer
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
- [ ] Create kernel upgrade script

## Usage
Command for update uboot from Fedora:
```
update-uboot --target=pinebook-pro-rk3399 --media=/dev/mmcblkX
```

## Supported Devices:
* Pinebook Pro

## Supported Editions / Desktops:
* Fedora Workstation<BR>
<BR>
There is no reason why this script should not work with other editions of Fedora. Just change FEDORAURL and FEDORARAW. Script os only tested with Fedora Workstation.

## Other notes:

This script **should** be distro-agnostic, which means you can install *pinebook-pro-fedora-installer* from **any** distro, as long as the dependencies are met.
