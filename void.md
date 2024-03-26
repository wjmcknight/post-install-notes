# Void Linux Post-Install Notes

As usual the basis for this is off the Void Linux XFCE live medium so some of
the packages installed are XFCE specific.

## Update System

```console
sudo xbps-install -Su
```

If you noticed the kernel was updated it wouldn't hurt to reboot here.

## Purge Old Kernel(s)

This may not be necessary but Void provides a handy tool for it. Void will
keep two kernels on your system but if you find there's a third one installed
the `vkpurge` will remove the oldest of them.

First let's see if there's anything that can be removed.

```console
sudo vkpurge list
```

The output of this shows available candidates for kernels that can be removed.
If this command doesn't output anything you're good to go. If it outputs
something like `5.10.17_1`, let's remove it.

```console
sudo vkpurge rm all
```

You can also remove it by specifying the version it outputs when listing
kernels instead of specifying `all`. That's the way to go if you have
multiple kernels that are candidates for removal but you don't want to purge
all of them.

## Enable nonfree Repository

We'll also switch to the a faster mirror then sync.

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.servercentral.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## AppArmor

For added security we'll install AppArmor here. I feel it's a better idea to
add and enable it here first before we install anything else.

```console
sudo xbps-install apparmor
sudo sed -i 's|GRUB_CMDLINE_LINUX_DEFAULT="loglevel=4"|GRUB_CMDLINE_LINUX_DEFAULT="loglevel=4 apparmor=1 security=apparmor"|' /etc/default/grub
sudo update-grub
```

## Install Packages

### Core

```console
sudo xbps-install vulkan-loader mesa-vulkan-radeon mesa-vaapi mesa-vdpau htop nano nmap tmux ncurses-term memtest86+ cronie xtools xdg-desktop-portal-gtk exfatprogs plocate gamin zsh vim gvim bluez blueman cpufrequtils haveged uptimed git base-devel python3-devel ruby-devel python3-pip ruby zola argyllcms xrdb galculator-gtk3 xfce4-places-plugin xfce4-pulseaudio-plugin xfce4-whiskermenu-plugin conky faba-icon-theme papirus-icon-theme greybird-themes
```

### Graphics and Printing

```console
sudo xbps-install cups cups-filters system-config-printer system-config-printer-udev gutenprint gimp inkscape feh scrot exiftool dcraw ImageMagick
```

### Internet

```console
sudo xbps-install chromium chromium-widevine filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
sudo xbps-install asunder audacity deadbeef guvcview handbrake xfburn beets python3-Flask mpg123 mpg123-pulseaudio mpv yt-dlp gstreamer-vaapi cmus cmus-faad cmus-ffmpeg cmus-flac cmus-pulseaudio eyeD3 libspa-bluetooth
```

### Office

```console
sudo xbps-install libreoffice xreader
```

## Virtualization

```console
sudo xbps-install libvirt qemu virt-manager
```

## Grant Access to kvm and libvirt Groups for Virtualization

```console
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
utilities.

## Flatpak

### Install and Enable

```console
sudo xbps-install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
```

## Set Default Cursor Theme

As this isn't defined, the login manager will show the ugly default Xorg
pointer. Since the XFCE media installs GNOME's Adwaita theme by default, let's
use that.

```console
sudo mkdir -p /usr/share/icons/default
echo -e "[Icon Theme]\nInherits=Adwaita" | sudo tee /usr/share/icons/default/index.theme
```

## Add User to bluetooth group

```console
sudo usermod -aG bluetooth yourusername
```

## Services

### Enable

```console
sudo ln -s /etc/sv/bluetoothd /var/service/
sudo ln -s /etc/sv/cronie /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/haveged /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
sudo ln -s /etc/sv/libvirtd /var/service/
sudo ln -s /etc/sv/virtlockd /var/service/
sudo ln -s /etc/sv/virtlogd /var/service/
```

### Disable

```console
sudo rm /var/service/sshd
```
