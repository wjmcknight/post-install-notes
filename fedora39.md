# Post-Install Notes: Fedora 39 XFCE

## Update System and Reboot

```console
sudo dnf update
```

## Enable RPM Fusion and openh264

```console
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf config-manager --enable fedora-cisco-openh264
```

## Install Some Packages

### Core

```console
sudo dnf install htop nmap memtest86+ zsh vim-X11 tuned haveged uptimed git python3-pip rubygems argyllcms icc-profiles-basiccolor-printing2009 icc-profiles-openicc conky papirus-icon-theme epapirus-icon-theme
```

### Graphics

```console
sudo dnf install gimp gimp-data-extras gimp-lensfun gutenprint-plugin inkscape feh scrot perl-Image-ExifTool dcraw
```

### Internet

```console
sudo dnf install chromium filezilla hexchat
```

### Multimedia

```console
sudo dnf install audacity-freeworld deadbeef guvcview HandBrake-gui beets ffmpeg-free mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 gstreamer1-plugins-bad-freeworld gstreamer1-plugins-good-extras gstreamer1-plugins-ugly cmus
```

### Virtualization

```console
sudo dnf install qemu-system-x86 libvirt-client libvirt-daemon virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Zola

```console
sudo dnf copr enable fz0x1/zola
sudo dnf install zola
```

### VSCodium

```console
sudo rpmkeys --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg
printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=download.vscodium.com\nbaseurl=https://download.vscodium.com/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg\nmetadata_expire=1h" | sudo tee -a /etc/yum.repos.d/vscodium.repo
sudo dnf install codium
```

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
sudo systemctl enable tuned --now
sudo systemctl enable uptimed --now
```
