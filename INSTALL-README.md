# Arch Installation

Hi! future Mithra! Follow this guide to perfectly install Arch linux onto your system. Read the instructions carefully and don't miss the Flagged ones! Enjoy Arching! :P

## Files Required

You can download Arch Linux (Latest) from this link ([https://www.archlinux.org/download/](https://www.archlinux.org/download/)) Select the nearest server for better download speeds.

## Flashing 

By using Rufus ([https://rufus.ie/](https://rufus.ie/)) or Balena Etcher ([https://www.balena.io/etcher/](https://www.balena.io/etcher/)) continue flashing!

## Partitioning the disks

For partitioning we can use fdisk `fdisk -l` 
> '-l' is for listing

You can see your drives in here. 

Now select the main disk which you want to edit the partitions

`fdisk /dev/sda` 

## Checking UEFI or Legacy file system


>If this directory exists then your system is EFI/UEFI enabled, if not then it is Legacy/MBR

`ls /sys/firmware/efi/efivars`

You need to create a +512M partition for UEFI. Skip if non-UEFI 

>Partition Type :- EFI System

## Create root partition (Both UEFI and Legacy)

You need to create a root(/) partition for the OS. You can just give default values for total space or just specify value for mannual approach.

>Use 'w' for writing changes or Ctrl + C for cancelling and re-allocating

## Creating Filesystem

For UEFI:-
`mkfs.fat -F32 /dev/sda1`
`mkfs.ext4 /dev/sda2`

For Legacy:-
`mkfs.ext4 /dev/sda1`

>If you have WiFi feature, connect WiFi by issuing $ wifi-menu

>Ping Google to verify connectivity.
>$ ping google.com

##  Select Pacman Server

`pacman -Syy`

`pacman -S reflector`

`cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak`

`reflector -c "*your country*" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist`

The closest pacman server is set! Great!


##  Installing Arch!
Mount the root partition that you have created by it s name ex:-(/dev/sda1)

`mount /dev/sda1 /mnt`

`pacstrap /mnt base linux linux-firmware vim nano sudo vi git`

Almost half-way done!

##  Configuring the installed System!

Generate fstab file by:-
`genfstab -U /mnt >> /mnt/etc/fstab`

Get into the arch system by:-
`arch-chroot /mnt`

##  Setting Timezone!

Find your timezone from here:-
`timedatectl list-timezones`

Apply the timezone from here:-
`timedatectl set-timezone *your timezone*`

Enable NTP:-
`timedatectl set-ntp true` 

##  Setting Locale! *important*!!!
>Before issuing any locale commands install sudo by :- `pacman -S sudo` just to make sure its installed. *Now editing the /etc/locale.gen file with nano, and un-commenting out your locale*.

THEN

`locale-gen`
verify the output!

`echo LANG=en_US.UTF-8 > /etc/locale.conf`

`export LANG=en_US.UTF-8`

##  Network Configuration!

Create a `/etc/hostname` file and add the hostname entry to this file.

`echo *hostname > /etc/hostname`

`touch /etc/hosts`

```
127.0.0.1	localhost
::1			localhost
127.0.1.1	*hostname
```
####  Set root password:- `passwd`

##  Creating User *also important

Check video ([https://www.youtube.com/watch?v=P4IV5BYPiPs](https://www.youtube.com/watch?v=P4IV5BYPiPs))

Adding user:-
`useradd -m -g users -G wheel *username`

Password for the user:-
`passwd *username`


###  Sudoers!
Before moving to the next step make sure these packages are installed:- `sudo` `vim` `vi`

Now issue the command:-
`visudo`
>Add this line (`*username ALL=(ALL) ALL`) next to the line (`root ALL=(ALL) ALL`)

##  Install Graphic Drivers!
To install the graphic driver you can do it right from here but I recommend doing it after installation. If you want to continue do it from here do this:-

AMD :- `pacman -S xf86-video-amdgpu`
NVIDIA :- `pacman -S xf86-video-nouveau`
Intel :- `pacman -S xf86-video-intel`

##  Install Grub bootloader

####  For UEFI :-
>Make sure you are still in arch-chroot. Packages:-

`pacman -S grub efibootmgr`
>Create a directory where EFI will be mounted

`mkdir /boot/efi`
>Now, mount the ESP partition

`mount /dev/sda1 /boot/efi`
>Install grub "like" this yours may differ!

`grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi`
>Last Step!
>
`grub-mkconfig -o /boot/grub/grub.cfg`

####  Now Legacy Systems:- 

>Install packages

`pacman -S grub`
>Don't put the number like sda1, just the disk name!

`grub-install /dev/sda` (The bootloader disk)
>Last Step

`grub-mkconfig -o /boot/grub/grub.cfg`

###  Almost there!
From here every installation may vary.

##  Installing Desktop Environment!

####  GNOME installation:-

>Installing Xorg:-

`pacman -S xorg`

>Install GNOME itself:-

`pacman -S gnome`

After installing everything till here. You need to start the services of them by issuing the commands:-

```
systemctl start gdm.service
systemctl enable gdm.service
systemctl enable NetworkManager.service
```

Now exit from chroot with the command `exit`

and shutdown the system by the command `shutdown now`

## Recommended Tips for a perfect installation (Post Install)
Install home directories with `sudo pacman -S xdg-user-dirs`

Audio functions to work properly you need to install ```sudo pacman -S pulseaudio pavucontrol pulseaudio-alsa alsa-utils```

Installing Yaourt (Temporarily)

`sudo nano /etc/pacman.conf`
add to bottom of file:
```
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

Installing pamac or yay any other AUR helpers.
pamac (GUI and NON-GUI) :- 
`yaourt -S pamac-aur`

yay (NON-GUI) (Recommended) :- 
package `git` required for installing.
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
Uncommenting multilib in `/etc/pacman.conf`


#### If something goes wrong dont worry! You can re-enter this exit scenario by again booting the live USB and mounting the `/dev/sda(linux partition)` to `/mnt` and using `arch-chroot /mnt` command to enter the system.


#### Things to do after installing:- https://github.com/Lend27/linuxstuff

Thanks!







