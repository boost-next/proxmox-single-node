Boot in rescue

installimage

use (unsupported) proxmox buster template

define settings for installationapt install 

reboot

ssh

rm /etc/apt/sources.list.d/hetzner-security-updates.list

vi /etc/apt/sources.list

deb http://mirror.hetzner.com/debian/packages bullseye main contrib non-free
deb http://mirror.hetzner.com/debian/packages bullseye-updates main contrib non-free
deb http://mirror.hetzner.com/debian/packages bullseye-backports main contrib non-free
deb http://mirror.hetzner.com/debian/security bullseye-security main contrib non-free

deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://deb.debian.org/debian bullseye-backports main contrib non-free
deb http://security.debian.org/debian-security bullseye-security main contrib non-free

# deb-src http://deb.debian.org/debian bullseye main contrib non-free
# deb-src http://deb.debian.org/debian bullseye-updates main contrib non-free
# deb-src http://deb.debian.org/debian bullseye-backports main contrib non-free
# deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free

vi /etc/kernel-img.conf

do_symlinks = no

# clean /vmlinuz /initr links and post-install and installimage

apt update

apt install screen randmac

apt purge 'linux-image-*'

apt autoremove --purge

apt dist-upgrade

sichern aktuelle Config

screen

(
 ifconfig ; 
 echo "" ; 
 echo -e "Random MAC: \c" ; 
 randmac -l | sed 's/../02/' ; 
 echo "" ; 
 ip route show ; 
 ip -6 route show ; 
 echo "" ; 
 cat /etc/network/interfaces
) > /root/network-originals.txt

> randmac -l -o 02
>
> The 02 for the first octet just sets the "locally assigned" bit, 
> which makes it obvious that it's not a vendor-provided MAC address, 
> and should guarantee that you won't collide with a real NIC's MAC address.

curl -L https://raw.githubusercontent.com/boost-next/proxmox-single-node/master/src/etc/network/interfaces > /etc/network/interfaces

vi /etc/network/interfaces

> disable gateway
> disable dns

curl -L https://raw.githubusercontent.com/boost-next/proxmox-single-node/master/src/usr/local/sbin/watchdog_ipv6_spoofed_mac_interface > /usr/local/sbin/watchdog_ipv6_spoofed_mac_interface

chmod 755 /usr/local/sbin/watchdog_ipv6_spoofed_mac_interface

vi /etc/default/pveproxy

# define a fix listener ip for pve web management
LISTEN_IP=192.168.123.1

vi /etc/hosts

# /etc/hosts file

# IPv6
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

# IPv4

127.0.0.1 localhost.localdomain localhost
192.168.123.1 pve.node.local pve

pveam update

pveam download local debian-11-standard_11.3-1_amd64.tar.zst
pveam download local ubuntu-22.04-standard_22.04-1_amd64.tar.zst

pveam available --section system

Download OpnSense DVD ISO

curl -L -O https://mirror.informatik.hs-fulda.de/opnsense/releases/22.7/OPNsense-22.7-OpenSSL-dvd-amd64.iso.bz2

bzip2 -d OPNsense-22.7-OpenSSL-dvd-amd64.iso.bz2

mv OPNsense-22.7-OpenSSL-dvd-amd64.iso /var/lib/vz/template/iso/


cfdisk /dev/sda

mkfs.btrfs /dev/sda5
mkfs.btrfs /dev/sda6

mkdir -p /vol/pve
mkdir -p /vol/data

mount /dev/sda5 /vol/pve
mount /dev/sda6 /vol/data

vi /etc/fstab

# create mirrors and filesystem

# stop pve

killall -9 corosync ; systemctl stop pve-cluster ; systemctl stop pvedaemon ; systemctl stop pvestatd ; systemctl stop pveproxy

mount /dev/sda3 /mnt
mount /dev/sda4 /vol/pve
mount /dev/sda5 /vol/data

btrfs device add -f /dev/sdb3 /mnt
btrfs device add -f /dev/sdb4 /vol/pve
btrfs device add -f /dev/sdb5 /vol/data

btrfs balance start -mconvert=raid1 -dconvert=raid1 /mnt
btrfs balance start -mconvert=raid1 -dconvert=raid1 /vol/pve
btrfs balance start -mconvert=raid1 -dconvert=raid1 /vol/data

btrfs balance start -musage=0 -dusage=0 /mnt
btrfs balance start -musage=0 -dusage=0 /vol/pve
btrfs balance start -musage=0 -dusage=0 /vol/data

btrfs filesystem show /
btrfs filesystem usage /

btrfs filesystem show /vol/pve
btrfs filesystem usage /vol/pve

btrfs filesystem show /vol/data
btrfs filesystem usage /vol/data

reboot



pvesm add btrfs pve --path /vol/pve
pvesm set pve --format subvol

pvesm add btrfs data --path /vol/data
pvesm set data --format subvol


Web config

Datacenter -> Storage -> local -> ISO Image | Container Template | Snippets
Datacenter -> Storage -> pve -> DISK Image | Container
Datacenter -> Storage -> data -> Container | VZDump backup file
pve -> System -> DNS -> DNS Server 1 -> 192.168.123.9
pve -> System -> Time -> Time zone





