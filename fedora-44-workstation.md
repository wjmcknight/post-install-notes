# Fedora 44 Post-Install Notes: Workstation (GNOME)

## Set hostname

```console
sudo hostnamectl hostname name.localdomain
```

## Set a couple DNF options

```console
echo -e "defaultyes=True\ninstall_weak_deps=False" | sudo tee -a /etc/dnf/dnf.conf
```

## Update system

```console
sudo dnf up
```

A reboot is almost guaranteed to be needed after updating.

## Enable RPMFusion

```console
sudo dnf in https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf up @core
sudo dnf up @multimedia
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf group install sound-and-video
```

### This step for the workstation

```console
sudo dnf in mesa-va-drivers-freeworld
```

### This step for the ThinkPad

```console
sudo dnf in ffmpeg-libs libva libva-utils
sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing
```

## Software

### Core

```console
sudo dnf in htop nmap ncurses-term memtest86+ zsh neovim pipx bat fzf fastfetch gnome-tweaks alacritty mint-y-icons
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
sudo dnf in audacity-freeworld beets flac lame mpg123 mpg123-plugins-pulseaudio mpv normalize python3-eyed3 yt-dlp
```

### Virtualization

```console
sudo dnf in virt-manager
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
sudo systemctl enable --now libvirtd
```

### Disable

```console
sudo systemctl disable --now bluetooth
sudo systemctl disable --now firewalld
sudo systemctl disable --now iscsi-onboot
sudo systemctl disable --now iscsi-starter
```
