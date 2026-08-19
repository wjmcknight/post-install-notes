# Fedora 44 Post-Install Notes: Workstation (GNOME)

## Set hostname

```console
sudo hostnamectl set-hostname host.localdomain
```

## Set a couple DNF settings

```console
echo -e "defaultyes=True\ninstall_weak_deps=False" | sudo tee -a /etc/dnf/dnf.conf
```

## Update system and reboot

```console
sudo dnf up
```

## Enable RPMFusion

```console
sudo dnf in https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf up @core
sudo dnf up @multimedia
sudo dnf group install sound-and-video
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

### This step is for the ThinkPad:

```console
sudo dnf install ffmpeg-libs libva libva-utils
sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing
```

## Software

### Core

```console
sudo dnf in android-tools htop nmap memtest86+ zsh neovim pipx bat fzf fastfetch alacritty mint-y-icons gnome-tweaks
```

### Graphics

```console
sudo dnf in gimp gimp-data-extras perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo dnf in geary transmission-gtk
```

### Multimedia

```console
sudo dnf in audacity-freeworld beets flac lame mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 yt-dlp gstreamer1-vaapi cmus 
```

### Virtualization

```console
sudo dnf in qemu-tools virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Install Spotify, LocalSend, Amberol, and Extension Manager

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub io.bassi.Amberol
flatpak install flathub com.mattjakeman.ExtensionManager
```

### LibreWolf

```console
sudo dnf config-manager addrepo --from-repofile=https://repo.librewolf.net/librewolf.repo
sudo dnf in librewolf
```

## Services

### Enable

```console
sudo systemctl enable --now fstrim.timer
sudo systemctl enable --now libvirtd
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now iscsi-onboot
sudo systemctl disable --now iscsi-starter
```
