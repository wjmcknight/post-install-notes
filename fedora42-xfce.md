# Post-Install Notes: Fedora 42 XFCE

## Disable Installation of Weak Dependencies

```console
echo "install_weak_deps=False" | sudo tee -a /etc/dnf/dnf.conf
```

## Update System and Reboot

```console
sudo dnf update
```

## Enable RPM Fusion

```console
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## Install Some Packages

### Core

```console
sudo dnf install htop nmap memtest86+ zsh vim-X11 tuned haveged uptimed git ncurses-term python3-pip rubygems argyllcms icc-profiles-basiccolor-printing2009 icc-profiles-openicc conky rofi papirus-icon-theme epapirus-icon-theme numix-gtk-theme numix-icon-theme numix-icon-theme-square
```

### Graphics

```console
sudo dnf install gimp gimp-data-extras inkscape feh scrot dcraw
```

### Internet

```console
sudo dnf install chromium filezilla
```

### Multimedia

```console
sudo dnf install audacity-freeworld guvcview HandBrake-gui beets ffmpeg-free mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 gstreamer1-plugins-bad-freeworld gstreamer1-plugins-good-extras gstreamer1-plugins-ugly mpd mpc ncmpcpp
```

### Virtualization

```console
sudo dnf install qemu libvirt virt-manager
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
sudo dnf install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Spotify

```console
flatpak install flathub com.spotify.Client
```

### Install Zola

```console
flatpak install flathub org.getzola.zola
```

## Services

### Disable

```console
sudo systemctl disable bluetooth --now
sudo systemctl disable firewalld --now
sudo systemctl disable iscsi-onboot --now
sudo systemctl disable iscsi-starter --now
```

### Enable

```console
sudo systemctl enable haveged --now
sudo systemctl enable libvirtd --now
sudo systemctl enable tuned --now
sudo systemctl enable uptimed --now
```
