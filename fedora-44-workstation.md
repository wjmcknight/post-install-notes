# Fedora 44 Post-Install Notes: Workstation (GNOME)

## Set hostname

```console
sudo hostnamectl hostname yournondefaulthostnamehere
```

## Make a few DNF tweaks and update system

```console
echo -e "defaultyes=True\ninstall_weak_deps=False" | sudo tee -a /etc/dnf/dnf.conf
sudo dnf update
```

It's probably a good idea to reboot after updating here.

## Enable RPM Fusion and switch to non-free ffmpeg

```console
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf update @core
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf group upgrade multimedia
```

## Software

### Core

```console
sudo dnf install android-tools htop nmap memtest86+ zsh neovim haveged uptimed pipx bat fzf fastfetch gnome-terminal gnome-tweaks
```

### Graphics

```console
sudo dnf install gimp gimp-data-extras gimpfx-foundry perl-Image-ExifTool dcraw
```

### Internet

```console
sudo dnf install filezilla geary transmission-gtk
```

### Multimedia

```console
sudo dnf install audacity-freeworld beets python3-flask flac lame mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 yt-dlp gstreamer1-vaapi cmus 
```

### Virtualization

```console
sudo dnf install qemu-tools virt-manager
```

#### Grant access to libvirt group

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running
libvirt tools.

### Flatpak

#### Enable

```console
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Install Amberol, Spotify, LocalSend, and Extension Manager

```console
flatpak install flathub io.bassi.Amberol
flatpak install flathub com.spotify.Client
flatpak install flathub org.localsend.localsend_app
flatpak install flathub com.mattjakeman.ExtensionManager
```

### LibreWolf

```console
sudo dnf config-manager addrepo --from-repofile=https://repo.librewolf.net/librewolf.repo
sudo dnf install librewolf
```

## Services

### Enable

```console
sudo systemctl enable --now fstrim.timer
sudo systemctl enable --now haveged
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
