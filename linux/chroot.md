# Chroot
## Debootstrap
### Install binutils and debootstrap
  apt-get install binutils debootstrap
### Create a folder for chroot
  mkdir -p /opt/chroot/myjail
### Clone an installation of a linux distro into the jail folder 
  debootstrap --arch  amd64 squeeze /opt/chroot/myjail http://http.debian.net/debian
### Mount with bind option
  mount --bind /proc/ /opt/chroot/jessie/proc/
  mount --bind /sys/ /opt/chroot/jessie/sys/
  mount --bind /dev/ /opt/chroot/jessie/dev/
  mount --bind /dev/pts/ /opt/chroot/jessie/dev/pts/
### chroot it
  chroot /opt/chroot/myjail 
