# Void Linux Post-Install Notes: GNOME

## Switch to faster mirror 

```console
sudo cp /usr/share/xbps.d/00-repository-main.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/00-repository-main.conf
```

## Add nonfree repo, switch mirror, Sync

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## Software

### Core

```console
sudo xbps-install linux-lts linux-firmware apparmor android-tools android-udev-rules xorg gnome pipewire avahi cronie cups cups-browsed cups-filters nss-mdns system-config-printer system-config-printer-udev htop ncurses-term nano nmap tmux memtest86+ plocate zsh neovim haveged uptimed git python3-pipx argyllcms alacritty bat fzf fastfetch gvfs-mtp
```

#### Enable LTS kernel

```console
sudo xbps-pkgdb -m manual linux-base
echo "ignorepkg=linux" | sudo tee /etc/xbps.d/20-linux-lts.conf
```

After rebooting to the LTS kernel we can remove the default kernel:

```console
sudo xbps-remove linux linux6.18
sudo vkpurge rm all
```

### Enable AppArmor

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

#### Add user to `network` group to control NetworkManager

```console
sudo usermod -aG network $(whoami)
```

### Graphics

```console
sudo xbps-install gimp inkscape exiftool dcraw
```

### Internet

```console
sudo xbps-install chromium chromium-widevine firefox-esr filezilla geary gnome-browser-connector transmission-gtk
```

### Multimedia

```console
sudo xbps-install amberol audacity beets python3-Flask ffmpeg flac mpg123 mpg123-pulseaudio mpv eyeD3 yt-dlp
```

### Office

```console
sudo xbps-install libreoffice
```

### Virtualization

```console
sudo xbps-install qemu-system-amd64 libvirt virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Flatpak

#### Install and enable

```console
sudo xbps-install flatpak gnome-software
```

After rebooting we run this command since at this point we're still
in the console without D-Bus running yet:

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install Spotify, LocalSend and LibreWolf

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub io.gitlab.librewolf-community
```

## Services

### Disable

```console
sudo rm /var/service/dhcpcd
```

If using a laptop or wireless in general that was enabled during
installation, you'll want to disable `wpa_supplicant` as it will clash
with `NetworkManager`:

```console
sudo rm /var/service/wpa_supplicant
```

### Enable

```console
sudo ln -s /etc/sv/dbus /var/service/
sudo ln -s /etc/sv/avahi-daemon /var/service/
sudo ln -s /etc/sv/cronie /var/service/
sudo ln -s /etc/sv/NetworkManager /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/cups-browsed /var/service/
sudo ln -s /etc/sv/gdm /var/service/
sudo ln -s /etc/sv/haveged /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
sudo ln -s /etc/sv/libvirtd /var/service/
sudo ln -s /etc/sv/virtlockd /var/service/
sudo ln -s /etc/sv/virtlogd /var/service/
```

### Enable fstrim for SSDs

```console
echo -e '#!/usr/bin/sh\n\nfstrim -A' | sudo tee /etc/cron.weekly/fstrim
sudo chmod 755 /etc/cron.weekly/fstrim
```
