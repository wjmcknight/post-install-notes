# Fedora 37: XFCE Post-Install Notes

## Installation

As an XFCE user I don't actually like the default setup of Fedora's XFCE spin.
What I do like are Debian and Void's sparse XFCE. To achieve a similar setup
under Fedora we install from the Server media and under `Software Selection`
we choose `Xfce Desktop` in the left side pane while leaving everything in the
right pane unselected.

## Enable RPMFusion

```console
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## Install Some Packages

### Core

```console
sudo dnf install iucode-tool htop nmap tmux memtest86+ zsh vim-X11 tuned haveged uptimed bzr git python3-pip rubygems hugo argyllcms icc-profiles-basiccolor-printing2009 icc-profiles-openicc xfce4-whiskermenu-plugin rxvt-unicode rofi conky moka-icon-theme
```

### Graphics

```console
sudo dnf install gimp gimp-data-extras gimp-lensfun gutenprint-plugin inkscape ristretto feh scrot perl-Image-ExifTool dcraw
```

### Internet

```console
sudo dnf install chromium-freeworld filezilla firefox geary hexchat transmission-gtk
```

### Multimedia

```console
sudo dnf install asunder audacious audacious-plugins-freeworld audacity-freeworld guvcview HandBrake-gui beets beets-plugins ffmpeg flac lame mpg123 mpg123-plugins-pulseaudio mpv python3-eyed3 youtube-dl gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly gstreamer1-vaapi cmus
```

### Virtualization

#```console
#sudo apt install qemu-system libvirt-clients libvirt-daemon-system virt-manager
#```

## Grant Access to libvirt Group for Virtualization

#```console
#sudo usermod -aG libvirt yourusername
#```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Flatpak

### Install and Enable

```console
sudo dnf install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal and Spotify

```console
flatpak install flathub org.signal.Signal
flatpak install flathub com.spotify.Client
```

## Install Papirus Icon Themes

```console
sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

## Services

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now sshd
```

### Enable

```console
sudo systemctl enable --now haveged
sudo systemctl enable --now tuned
sudo systemctl enable --now uptimed
```
