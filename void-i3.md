# Void Linux Post-Install Notes: i3

We're going to start with a base install here as well as doing a remote
packages install that way upon reboot we're already up to date.

## Create Regular User

There appears to be a bug in the installer where despite specifying the
credentials you'd like to use for your regular user, these don't get applied
by the installer so we do this manually during the first boot.

```console
useradd -m -G wheel,disk,lp,audio,video,optical,storage,kvm,input,plugdev -c "Real Name" user
passwd user
```

Next we uncomment the following line to enable `sudo` access when running
`visudo`:

```console
# %wheel ALL=(ALL:ALL) ALL
```

Logging out of root then logging back in as the user you created should now
have proper `sudo` privileges.

## WiFi

```
wpa_passphrase [ssid] [key] | sudo tee -a /etc/wpa_supplicant/wpa_supplicant.conf
sudo ln -s /usr/share/dhcpcd/hooks/10-wpa_supplicant /usr/libexec/dhcpcd-hooks
sudo ln -s /etc/sv/dhcpcd /var/service/
sudo sv restart dhcpcd
```

## Enable nonfree Repository

```console
sudo xbps-install void-repo-nonfree
```

## Switch to a Better Mirror and Sync

```console
sudo cp /usr/share/xbps.d/00-repository-main.conf /etc/xbps.d/
sudo cp /usr/share/xbps.d/10-repository-nonfree.cinf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.servercentral.com/voidlinux|' /etc/xbps.d/*.conf
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
sudo xbps-install intel-ucode htop nano nmap tmux memtest86+ plocate gamin zsh vim gvim chrony cpufrequtils haveged uptimed xorg i3 i3lock i3status dmenu lxdm elogind polkit pulseaudio pavucontrol pasystray NetworkManager network-manager-applet xfce4-settings alacritty Thunar thunar-volman nitrogen picom bzr git base-devel python3-pip ruby hugo argyllcms xautolock xrdb galculator-gtk3 gvfs gvfs-mtp faba-icon-theme papirus-icon-theme greybird-themes
```

### Services and Reboot

This is a good time to enable a few services needed for a graphical log in as
well as authentication and reboot

```console
sudo rm /var/service/dhcpcd
sudo ln -s /etc/sv/chronyd /var/service/
sudo ln -s /etc/sv/dbus /var/service/
sudo ln -s /etc/sv/elogind /var/service/
sudo ln -s /etc/sv/haveged /var/service/
sudo ln -s /etc/sv/lxdm /var/service/
sudo ln -s /etc/sv/NetworkManager /var/service/
sudo ln -s /etc/sv/polkitd /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
```

### Graphics and Printing

```console
sudo xbps-install cups cups-filters system-config-printer system-config-printer-udev gutenprint gimp inkscape ristretto feh scrot exiftool dcraw
```

### Internet

```console
sudo xbps-install chromium chromium-widevine filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
sudo xbps-install asunder audacious audacious-plugins audacity guvcview handbrake parole xfburn beets mpg123 mpg123-pulseaudio mpv youtube-dl gst-plugins-good1 gstreamer-vaapi cmus cmus-faad cmus-ffmpeg cmus-flac cmus-pulseaudio eyeD3
```

### Office

```console
sudo xbps-install abiword gnumeric xreader
```

### Virtualization

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

## Remaining Services

```console
sudo ln -s /etc/sv/cups-browsed /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/libvirtd /var/service/
sudo ln -s /etc/sv/virtlockd /var/service/
sudo ln -s /etc/sv/virtlogd /var/service/
```
