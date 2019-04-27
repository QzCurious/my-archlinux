## check efi mode

```
is_uefi_mode() {
    if ls /sys/firmware/efi/efivars > /dev/null; then
        echo 1
    else
        echo 0
    fi
}
```

## wifi

```
wpa_supplicant -B -i interface -c <(wpa-passphase SSID PASS)
```

```
systemctl restart dhcpcd
```

## time

timedatectl set-ntp true

## Partition (gdisk)

| mount point | code |
| ----------- | ---- |
| /           | 8304 |
| /boot/efi   | ef00 |
| /home       | 8302 |
| /swap       | 8200 |

## Filesystem

| mount point | filesystem |
| ----------- | ---------- |
| /           | mkfs.ext4  |
| /boot/eft   | mkfs.vfat  |
| /home       | mkfs.ext4  |
| /swap       | mkswap     |

## Mount partitions to /mnt

## Select mirror

```
/etc/pacman.d/mirrorlist
========================

## Country : Taiwan
Server = http://free.nchc.org.tw/manjaro/stable/$repo/$arch
```

## Install

```
pacstrap /mnt base base-devel intel-ucode vim
```

## fstab

```
genfstab -U /mnt >> /mnt/etc/fstab
```

## Setup

```
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
hwclock --systohc
sed 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/'
```

```
/etc/locale.conf
================

LANG=en_US.UTF-8
LC_ADDRESS=en_US.UTF-8
LC_IDENTIFICATION=en_US.UTF-8
LC_MEASUREMENT=en_US.UTF-8
LC_MONETARY=en_US.UTF-8
LC_NAME=en_US.UTF-8
LC_NUMERIC=en_US.UTF-8
LC_PAPER=en_US.UTF-8
LC_TELEPHONE=en_US.UTF-8
LC_TIME=en_US.UTF-8
```

```
/etc/hostname
=============

qz-pc
```

```
/etc/hosts
=========

127.0.0.1       localhost
::1             localhost
127.0.1.1       qz-pc.localhost  qz-pc
```

## Boot loader (GRUB)

```
pacman -S --noconfirm grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

## Root passward

```
passwd
```

## User

```
useradd -m -d /home/qz -G wheel audio input lp sys network power qz
(-G bumblebee)
```

## Install packages

```
pacman -S --noconfirm networkmanager bash-completion ntfs-3g xorg plasma obs-studio dolphin ark
```

```
systemctl enable sddm
systemctl enable NetworkManager
```

## Reboot

```
sudo passwd -l root
```

```
/etc/sysctl.d/99-sysctl.conf
============================

vm.swappiness=0
```

## yay

```
cd /tmp
git clone https://aur.archlinux.org/yay.git yay
cd yay
makepkg -si

cd ..
rm -rf yay
```

## Packages

_/etc/pacman.d/mirrorlist_

`pacman -S --needed --noconfirm`

