# Devuan Daedalus Post-Install Notes

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main non-free-firmware|main non-free-firmware non-free contrib|' /etc/apt/sources.list
```

## Enable Backports

```console
echo -e "# Backports\ndeb http://deb.devuan.org/merged daedalus-backports main non-free-firmware non-free contrib\ndeb-src http://deb.devuan.org/merged daedalus-backports main non-free-firmware non-free contrib" | sudo tee /etc/apt/sources.list.d/backports.list
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Install Some Packages

### Core

```console
sudo apt install firmware-linux-nonfree firmware-realtek amd64-microcode htop nmap tmux memtest86+ plocate gamin zsh vim vim-gtk3 tuned haveged uptimed git build-essential aptitude sysv-rc-conf python3-pip ruby-rubygems hugo argyll icc-profiles conky-std galculator gvfs-backends xfce4-indicator-plugin faba-icon-theme moka-icon-theme papirus-icon-theme greybird-gtk-theme
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras gimp-gutenprint gimp-lensfun inkscape feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install chromium filezilla geary hexchat transmission-gtk
```

### Multimedia

```console
sudo apt install asunder audacious audacity guvcview handbrake beets ffmpeg flac lame mpg123 mpv normalize-audio eyed3 yt-dlp gstreamer1.0-vaapi cmus
```

### Virtualization

```console
sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
```

## Manage Services

```console
sudo sysv-rc-conf
```
