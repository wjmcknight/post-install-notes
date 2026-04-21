# Arch Linux Installation Notes: Xfce

## Installation

Since these will potentially vary depending on my use, we start at the point 
of `pacstrap`:

```console
pacstrap -K /mnt base linux-lts linux-firmware amd-ucode grub cronie nano sudo networkmanager xorg xf86-video-amdgpu vulkan-radeon lightdm lightdm-gtk-greeter xfce4 xfce4-goodies pipewire pipewire-pulse wireplumber pavucontrol network-manager-applet ttf-dejavu ttf-droid gnu-free-fonts ttf-input-nerd noto-fonts ttf-roboto ttf-croscore ttf-liberation cantarell-fonts
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Canada/Eastern /etc/localtime
hwclock --systohc
sed -i 's|#en_CA.UTF-8 UTF-8|en_CA.UTF-8 UTF-8|' /etc/locale.gen
locale-gen
echo "LANG=en_CA.UTF-8" | tee /etc/locale.conf
echo "somehostname" | tee /etc/hostname
passwd
useradd -m -G wheel -s /usr/bin/bash -c "Your Name" youruser
passwd youruser
visudo
systemctl enable cronie
systemctl enable NetworkManager
systemctl enable lightdm
systemctl enable systemd-timesyncd
grub-install --target=i386-pc /dev/vda
grub-mkconfig -o /boot/grub/grub.cfg
```

## Software

### Core

```console
sudo pacman -S android-tools htop nmap tmux memtest86+ plocate zsh gvim haveged uptimed git python-pip argyllcms bat fzf fastfetch galculator gvfs gvfs-mtp gvfs-nfs gvfs-smb papirus-icon-theme
```

### Graphics

```console
sudo pacman -S gimp inkscape feh scrot perl-image-exiftool dcraw
```

### Internet

```console
sudo pacman -S chromium firefox filezilla transmission-gtk
```

### Multimedia

```console
sudo pacman -S tenacity guvcview handbrake pragha beets python-flask mpv wavegain yt-dlp gst-plugins-bad gst-plugins-ugly mpd mpc ncmpcpp
```

### Virtualization

```console
sudo pacman -S qemu-desktop virt-manager
```

#### Grant Access to libvirt Group for Virtualization

```console
sudo usermod -aG libvirt $(whoami)
```

A logout is needed here to reflect the permission changes for running libvirt
tools.

### Flatpak

#### Install and Enable

```console
sudo pacman -S flatpak xdg-desktop-portal-gtk
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

A reboot is needed before being able to install anything from Flatpak.

#### Install Spotify, Zola, and LocalSend

```console
flatpak install flathub com.spotify.Client
flatpak install flathub org.getzola.zola
flatpak install flathub org.localsend.localsend_app
```

## Services

### Enable

```console
sudo systemctl enable fstrim.timer --now
sudo systemctl enable haveged --now
sudo systemctl enable libvirtd --now
sudo systemctl enable uptimed --now
```
