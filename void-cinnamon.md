# Void Linux Post-Install Notes: Cinnamon

## Switch to Better Mirror

```console
sudo cp /usr/share/xbps.d/00-repository-main.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/00-repository-main.conf
```

## Add nonfree Repo, Switch Mirror, Sync

```console
sudo xbps-install void-repo-nonfree
sudo cp /usr/share/xbps.d/10-repository-nonfree.conf /etc/xbps.d/
sudo sed -i 's|repo-default.voidlinux.org|mirrors.summithq.com/voidlinux|' /etc/xbps.d/10-repository-nonfree.conf
sudo xbps-install -S
```

## Software

### Core

```console
sudo xbps-install linux-lts linux-firmware apparmor android-tools android-udev-rules xorg cinnamon-all lightdm lightdm-slick-greeter pulseaudio avahi cronie cups cups-browsed system-config-printer system-config-printer-udev htop ncurses-term nano nmap memtest86+ plocate zsh neovim haveged uptimed git python3-pipx argyllcms gedit seahorse gnome-calculator alacritty bat fzf fastfetch gvfs-mtp
```

#### Enable LTS Kernel

```console
sudo xbps-pkgdb -m manual linux-base
echo "ignorepkg=linux" | sudo tee /etc/xbps.d/20-lts.conf
```

After rebooting to the LTS kernel we can remove the default kernel:

```console
sudo xbps-remove linux linux6.18
sudo vkpurge rm all
```

#### Enable AppArmor

```console
sudo sed -i 's|loglevel=4|loglevel=4 apparmor=1 security=apparmor|' /etc/default/grub
sudo update-grub
```

#### Enable lightdm-slick-greeter

```console
sudo sed -i 's|#greeter-session=example-gtk-gnome|greeter-session=slick-greeter|' /etc/lightdm/lightdm.conf
```

### Graphics

```console
sudo xbps-install eog gimp inkscape shotwell feh exiftool dcraw
```

### Internet

```console
sudo xbps-install chromium chromium-widevine firefox-esr filezilla transmission-gtk
```

### Multimedia

```console
sudo xbps-install audacity brasero celluloid cheese deadbeef sound-juicer beets python3-Flask ffmpeg flac mpg123 mpg123-pulseaudio mpv eyeD3 yt-dlp
```

### Office

```console
sudo xbps-install libreoffice
```

### Virtualization

```console
sudo xbps-install qemu-system-amd64 libvirt virt-manager
```

#### Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Flatpak

#### Install and Enable

```console
sudo xbps-install flatpak xdg-desktop-portal-gtk
```

After rebooting we run this command since at this point we're still
in the console without D-Bus running yet:

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install Spotify, Zola, LocalSend, and LibreWolf

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
flatpak install flathub org.localsend.localsend_app
flatpak install flathub io.gitlab.librewolf-community
```

## Services

### Disable

```console
sudo rm /var/service/dhcpcd
```

### Enable

```console
sudo ln -s /etc/sv/dbus /var/service/
sudo ln -s /etc/sv/avahi-daemon /var/service/
sudo ln -s /etc/sv/cronie /var/service/
sudo ln -s /etc/sv/NetworkManager /var/service/
sudo ln -s /etc/sv/cupsd /var/service/
sudo ln -s /etc/sv/cups-browsed /var/service/
sudo ln -s /etc/sv/lightdm /var/service/
sudo ln -s /etc/sv/haveged /var/service/
sudo ln -s /etc/sv/uptimed /var/service/
```
