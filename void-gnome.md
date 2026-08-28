# Void Linux Post-Install Notes: GNOME

## Enable non-free repo then sync

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## Software

### Core

```console
sudo xbps-install linux-firmware apparmor base-devel xtools xorg xdg-user-dirs-gtk gnome pipewire avahi chrony cronie cups cups-browsed cups-filters nss-mdns system-config-printer system-config-printer-udev htop nmap tmux ncurses-term memtest86+ plocate zsh neovim wget ntfs-3g python3-pipx bat fzf fastfetch alacritty gvfs-mtp gedit gedit-plugins gnome-screenshot seahorse extension-manager
```

#### Enable apparmor

```console
sudo sed -i 's|loglevel=4|loglevel=4 apparmor=1 security=apparmor|' /etc/default/grub
sudo update-grub
```

#### Enable Pipewire

```console
sudo mkdir -p /etc/pipewire/pipewire.conf.d
sudo ln -s /usr/share/examples/wireplumber/10-wireplumber.conf /etc/pipewire/pipewire.conf.d/
sudo ln -s /usr/share/examples/pipewire/20-pipewire-pulse.conf /etc/pipewire/pipewire.conf.d/
sudo ln -s /usr/share/applications/pipewire.desktop /etc/xdg/autostart/
```

#### Prefer libinput over synaptics on ThinkPad

```console
sudo mkdir -p /etc/X11/xorg.conf.d
sudo ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/
```

### Graphics

```console
sudo xbps-install gimp shotwell exiftool dcraw libwebp-tools ImageMagick
```

### Internet

```console
sudo xbps-install firefox-esr geary transmission-gtk
```

### Multimedia

```console
sudo xbps-install amberol audacity beets ffmpeg flac mpg123 mpg123-pulseaudio mpv eyeD3 yt-dlp
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

#### Install

```console
sudo xbps-install flatpak
```

#### Enable

Since we're still in console without dbus running we'll run this once we
reboot.

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install LibreWolf, Spotify, and LocalSend

```console
flatpak install flathub io.gitlab.librewolf-community
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
```

## Services

### Disable

```console
sudo rm /var/service/dhcpcd
```

If we installed using wireless we should disable wpa_supplicant since it
will conflict with NetworkManager.

```console
sudo rm /var/service/wpa_supplicant
```

### Enable

```console
sudo ln -s /etc/sv/dbus/ /var/service
sudo ln -s /etc/sv/NetworkManager/ /var/service
sudo ln -s /etc/sv/chronyd/ /var/service
sudo ln -s /etc/sv/cronie/ /var/service
sudo ln -s /etc/sv/cupsd/ /var/service
sudo ln -s /etc/sv/avahi-daemon/ /var/service
sudo ln -s /etc/sv/gdm/ /var/service
sudo ln -s /etc/sv/libvirtd/ /var/service
sudo ln -s /etc/sv/virtlockd/ /var/service
sudo ln -s /etc/sv/virtlogd/ /var/service
```

### fstrim

Since Void doesn't use systemd we'll enable fstrim using cron.

```console
echo -e '#!/bin/sh\nfstrim -A' | sudo tee /etc/cron.weekly/fstrim
sudo chmod 755 /etc/cron.weekly/fstrim
```
