# Debian Trixie Post-Install Notes: GNOME

## Enable contrib and non-free Repos

```console
sudo sed -i 's|main non-free-firmware|main non-free-firmware non-free contrib|' /etc/apt/sources.list
```

## Switch Security Repo to Better Mirror

```console
sudo sed -i 's|security.debian.org|mirror.csclub.uwaterloo.ca|' /etc/apt/sources.list
```

## Enable Deb Multimedia

```console
wget http://www.deb-multimedia.org/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2024.9.1_all.deb
sudo dpkg -i deb-multimedia-keyring_2024.9.1_all.deb
echo -e "# Multimedia\ndeb http://mirror.csclub.uwaterloo.ca/debian-multimedia/ trixie main non-free\ndeb-src http://mirror.csclub.uwaterloo.ca/debian-multimedia/ trixie main non-free" | sudo tee /etc/apt/sources.list.d/multimedia.list
```

## Enable Backports

```console
echo -e "# Backports\ndeb http://mirror.csclub.uwaterloo.ca/debian/ trixie-backports main non-free-firmware non-free contrib\ndeb-src http://mirror.csclub.uwaterloo.ca/debian/ trixie-backports main non-free-firmware non-free contrib" | sudo tee /etc/apt/sources.list.d/backports.list
```

## Update System

```console
sudo apt update
sudo apt full-upgrade
```

## Install Some Packages

### Core

```console
sudo apt install bluez-firmware firmware-linux amd64-microcode htop nmap tmux memtest86+ plocate zsh vim vim-gtk3 haveged uptimed git aptitude build-essential python3-pip argyll icc-profiles bat fzf fastfetch gnome-shell-extension-dashtodock gnome-shell-extension-manager gnome-shell-extension-user-theme mint-y-icons
```

### Graphics

```console
sudo apt install create-resources gimp gimp-data-extras inkscape feh scrot libimage-exiftool-perl dcraw
```

### Internet

```console
sudo apt install chromium filezilla transmission-gtk
```

### Multimedia

```console
sudo apt install audacity brasero handbrake-gtk sound-juicer beets python3-flask ffmpeg flac lame mpg123 mpv normalize-audio eyed3 yt-dlp gstreamer1.0-vaapi mpd mpc ncmpcpp 
```

### Virtualization

```console
sudo apt install virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
sudo apt install flatpak gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed before being able to install anything from Flatpak.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
```

## Services

### Enable

```console
sudo systemctl enable fstrim.timer --now
```

### Disable

```console
sudo systemctl disable mpd --now
sudo systemctl disable open-iscsi --now
```
