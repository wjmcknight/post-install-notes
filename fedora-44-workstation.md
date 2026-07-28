# Fedora 44 Post-Install Notes: Workstation (GNOME)

## Set hostname

```console
sudo hostnamectl hostname yourhostname
```

## Set a few dnf settings

```console
echo "defaultyes=True" | sudo tee -a /etc/dnf/dnf.conf
echo "install_weak_deps=False" | sudo tee -a /etc/dnf/dnf.conf
```

## Upgrade system

```console
sudo dnf up
```

You'll probably need to reboots after the upgrade complete.

## Enable RPMFusion

```console
sudo dnf in https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf up @core
sudo dnf up @multimedia
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf swap mesa-vulkan-drivers mesa-vulkan-drivers-freeworld --allowerasing
```

## Software

### Core

```console
sudo dnf in android-tools htop nmap ncurses-term memtest86+ zsh neovim uptimed pipx bat fzf fastfetch alacritty mint-y-icons gnome-tweaks
```

### Graphics

```console
sudo dnf in gimp gimp-data-extras perl-Image-ExifTool dcraw libwebp-tools
```

### Internet

```console
sudo dnf in filezilla geary transmission-gtk
```

### Multimedia

```console
sudo dnf in audacity-freeworld beets flac lame mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 yt-dlp cmus 
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

#### Install Spotify, LocalSend, Extension Manager, and Amberol

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub com.mattjakeman.ExtensionManager
flatpak install flathub io.bassi.Amberol
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
sudo systemctl enable --now cups
sudo systemctl enable --now cups-browsed
sudo systemctl enable --now libvirtd
sudo systemctl enable --now uptimed
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now iscsi-onboot
sudo systemctl disable --now iscsi-starter
```
