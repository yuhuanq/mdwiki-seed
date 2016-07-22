# Useful terminal commands for practical uses

`-help` flag for summary of flags.

`man <command>` for manual pages.


## youtube-dl

To download audio:

```bash
$ youtube-dl --extract-audio --audio-quality 0 --audio-format best <url>
```


## Setting window controls

```bash
$ gsettings set org.gnome.desktop.wm.preferences button-layout 'close,maximize,minimize:'
```

Sets the controls to the top left


## Upgrading Bios

```bash
$ cat /sys/devices/virtual/dmi/id/bios_version
```

or `dmidecode` to see current bios_version

Download the bootable cd image from lenovo software/drivers webpage.

`sudo dnf install genisoimage`

`sudo dnf install geteltorito`

use `gelteltorito` to convert the ISO

`geteltorito -o bios.img gluj19us.iso`

Insert a USB stick you're willing to erase entirely and then copy the image onto it (replacing sdX
with the correct device name, not partition name, for the USB stick):

`dd if=bios.img of=/dev/sdX`

then restart and boot form the USB by pressing enter, then f12.

### Boot USBs in general

To create a bootable usb in general `dd if=<>.iso of=/dev/sdX`

To use `unetbootin` have to be su, however cannot run with simply `sudo unetbootin`.

Solution: `sudo unetbootin &; sudo QT_X11_NO_MITSHM=1 unetbootin`


## Adding Kernel boot parameters

For Fedora, append parameters to `linuxefi /vmlinuz-...` line at `/boot/efi/EFI/fedora/grub.cfg`



