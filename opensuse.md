# openSUSE 15.3 Post-Install Notes

## Disable Zypper's vendor check

```console
$ sudo sed -i 's|# solver.allowVendorChange = false|solver.allowVendorChange = true|' /etc/zypp/zypp.conf
```

## Enable the Packman repo

```console
$ sudo zypper ar -cfp 95 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_15.3/ packman
```

## Update System

```console
$ sudo zypper dup
$ sudo zypper up
```

## Install Some Packages

### Core

```console
$ sudo zypper in htop nmap tmux memtest86+ zsh git argyllcms icc-profiles-all perl-Image-ExifTool conky xfce4-places-plugin faba-icon-theme moka-icon-theme metatheme-greybird-common
```

### Graphics

```console
$ sudo zypper in gutenprint-gimpplugin inkscape
```

### Internet

```console
$ sudo zypper in chromium chromium-ffmpeg-extra chromium-plugin-widevinecdm filezilla hexchat
```

### Multimedia

```console
$ sudo zypper in asunder audacious audacity guvcview handbrake-gtk xfburn beets ffmpeg-4 flac lame mpg123 mpv normalize youtube-dl gstreamer-plugins-bad-orig-addon gstreamer-plugins-good-extra gstreamer-plugins-libav gstreamer-plugins-ugly gstreamer-plugins-ugly-orig-addon gstreamer-plugins-vaapi cmus
```

### Virtualization

```console
$ sudo zypper in qemu libvirt-client libvirt-daemon-qemu virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
$ sudo usermod -G libvirt yourusernamehere
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Install VSCodium

```console
$ sudo rpmkeys --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg
$ printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=gitlab.com_paulcarroty_vscodium_repo\nbaseurl=https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg" | sudo tee -a /etc/zypp/repos.d/vscodium.repo
$ sudo zypper ref
$ sudo zypper in codium
```

## Install Hugo

```console
$ sudo zypper addrepo https://download.opensuse.org/repositories/home:GNorth/openSUSE_Leap_15.2/home:GNorth.repo
$ sudo zypper ref
$ sudo zypper in hugo
```

## Flatpak

### Install and Enable

```console
$ sudo zypper in flatpak
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal, Spotify, and Telegram

```console
$ flatpak install flathub org.signal.Signal
$ flatpak install flathub com.spotify.Client
$ flatpak install flathub org.telegram.desktop
```

## Install Papirus Icon Themes

```console
$ sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
$ sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

## Services

### Disable

```console
$ sudo systemctl stop bluetooth
$ sudo systemctl disable bluetooth
$ sudo systemctl stop iscsi
$ sudo systemctl disable iscsi
```

### Enable

```console
$ sudo systemctl start libvirtd
$ sudo systemctl enable libvirtd
$ sudo systemctl start tuned
$ sudo systemctl enable tuned
```
