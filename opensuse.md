# openSUSE Leap 15.3 Post-Install Notes

## Enable Vendor Change

```console
sudo sed -i 's|# solver.allowVendorChange = false|solver.allowVendorChange = true|' /etc/zypp/zypp.conf
```

## Enable Packman Repo

```console
sudo zypper ar -cfp 95 http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Leap_15.3/ packman
```

## Update System

```console
sudo zypper up
```

## Install Some Packages

### Core

```console
sudo zypper in htop nmap tmux memtest86+ zsh bzr git-core hugo argyllcms icc-profiles-all feh scrot perl-Image-ExifTool conky faba-icon-theme moka-icon-theme metatheme-greybird-common
```

### Graphics

```console
sudo zypper in gutenprint-gimpplugin inkscape
```

### Internet

```console
sudo zypper in enigmail filezilla hexchat
```

### Multimedia

```console
sudo zypper in asunder audacious audacity guvcview handbrake-gtk beets ffmpeg-4 flac lame mpg123 mpv normalize btag youtube-dl gstreamer-plugins-ugly gstreamer-plugins-vaapi cmus
```

### Virtualization

```console
sudo zypper in qemu libvirt-client libvirt-daemon-qemu virt-manager
```

## Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt yourusername
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

## Install Brave

```console
sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc
sudo zypper ar https://brave-browser-rpm-release.s3.brave.com/x86_64/ brave-browser
sudo zypper in brave-browser
```

## Install VSCodium

```console
sudo rpmkeys --import https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg
printf "[gitlab.com_paulcarroty_vscodium_repo]\nname=gitlab.com_paulcarroty_vscodium_repo\nbaseurl=https://download.vscodium.com/rpms/\nenabled=1\ngpgcheck=1\nrepo_gpgcheck=1\ngpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/-/raw/master/pub.gpg\nmetadata_expire=1h" | sudo tee -a /etc/zypp/repos.d/vscodium.repo
sudo zypper in codium
```

## Install Hugo

```console
sudo zypper ar https://download.opensuse.org/repositories/home:GNorth/openSUSE_Leap_15.3/home:GNorth.repo
sudo zypper in hugo
```

## Flatpak

### Install and Enable

```console
sudo zypper in flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed here before being able to install anything from Flathub.

### Install Signal, Spotify, and Telegram

```console
flatpak install flathub org.signal.Signal
flatpak install flathub com.spotify.Client
flatpak install flathub org.telegram.desktop
```

## Install Papirus Icon Themes

```console
sudo wget -qO- https://git.io/papirus-icon-theme-install | sh
sudo wget -qO- https://raw.githubusercontent.com/PapirusDevelopmentTeam/papirus-filezilla-themes/master/install.sh | sh
```

## Services

### Enable

```console
sudo systemctl enable --now libvirtd
sudo systemctl enable --now tuned
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now iscsi
sudo systemctl disable --now postfix
```
