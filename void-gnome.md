# Void Linux Post-Install Notes: GNOME

## Enable non-free repo and switch to a faster mirror

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## Software

### Core

```console
sudo xbps-install linux-lts linux-firmware apparmor android-tools android-udev-rules base-devel xtools xorg xdg-user-dirs-gtk gnome pipewire avahi chrony cronie cups cups-browsed cups-filters nss-mdns system-config-printer system-config-printer-udev uptimed htop nano nmap tmux ncurses-term memtest86+ plocate zsh neovim wget ntfs-3g meson ninja python3-pipx alacritty bat fzf fastfetch gvfs-mtp gedit gedit-plugins gnome-screenshot
```

#### Set the LTS kernel as default kernel

```console
sudo xbps-pkgdb -m manual linux-base
echo "ignorepkg=linux" | sudo tee /etc/xbps.d/20-linux-lts.conf
```

Once we reboot into the LTS kernel we can remove the original.

```console
sudo xbps-remove linux linux6.18
sudo vkpurge rm all
```

#### Enable apparmor

```console
sudo sed -i 's|loglevel=4|loglevel=4 apparmor=1 security=apparmor|' /etc/default/grub
sudo update-grub
```

#### Enable pipewire

```console
sudo mkdir -p /etc/pipewire/pipewire.conf.d
sudo ln -s /usr/share/examples/wireplumber/10-wireplumber.conf /etc/pipewire/pipewire.conf.d/
sudo ln -s /usr/share/examples/pipewire/20-pipewire-pulse.conf /etc/pipewire/pipewire.conf.d/
sudo ln -s /usr/share/applications/pipewire.desktop /etc/xdg/autostart/
```

#### ThinkPad T14 Gen 3 touchpad tweak

When installing the `xorg` metapackage it includes both libinput and
synaptics drivers with synaptics taking precedence over libinput. We 
switch to libinput since synaptics behaviour feels clunky where libinput
is what Debian and Fedora default to and is a much nicer in terms of 
usability.

```console
sudo mkdir -p /etc/X11/xorg.conf.d
sudo ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/
```

### Graphics

```console
sudo xbps-install gimp eog eog-plugins shotwell exiftool dcraw libwebp-tools
```

### Internet

```console
sudo xbps-install firefox-esr geary filezilla transmission-gtk
```

### Multimedia

```console
sudo xbps-install amberol audacity blanket totem beets python3-Flask ffmpeg flac mpg123 mpg123-pulseaudio mpv eyeD3 yt-dlp
```

### Office

```console
sudo xbps-install libreoffice
```

### Virtualization

```console
sudo xbps-install qemu-system-amd64 qemu-img qemu-tools libvirt virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Install and enable

```console
sudo xbps-install flatpak
```

Since we're still in console and dbus isn't running we enable Flathub
after a reboot.

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install LibreWolf, Spotify, LocalSend, and Extension Manager

```console
flatpak install flathub io.gitlab.librewolf-community
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub com.mattjakeman.ExtensionManager
```

## Services

### Disable

```console
sudo rm /var/service/dhcpcd
```

If you used a wireless connection during installation we'll need to
disable wpa_supplicant as it will conflict with NetworkManager.

```console
sudo rm /var/service/wpa_supplicant
```

### Enable

```console
sudo ln -s /etc/sv/dbus /var/service/
sudo ln -s /etc/sv/NetworkManager /var/service/
sudo ln -s /etc/sv/chronyd /var/service/
sudo ln -s /etc/sv/cronie /var/service/
sudo ln -s /etc/sv/avahi-daemon /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/cups-browsed /var/service/
sudo ln -s /etc/sv/gdm /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
sudo ln -s /etv/sv/libvirtd /var/service/
sudo ln -s /etc/sv/virtlockd /var/service/
sudo ln -s /etc/sv/virtlogd /var/service/
```

#### fstrim for solid state drives

Since this isn't handled by a service in Void we handle it using cron.

```console
echo -e '#!/bin/sh\nfstrim -A' | sudo tee /etc/cron.weekly/fstrim
sudo chmod 755 /etc/cron.weekly/fstrim
```
