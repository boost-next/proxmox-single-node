After installing proxmox following package can be purged.

But make sure to re-run service anabling afterwards

```bash
apt purge ifupdown
systemctl enable networking
# or does the same
apt install --reinstall ifupdown2
```



## stop all proxmox service except cluser for /etc/pve mount

```bash
systemctl stop pvebanner.service pveproxy.service pvefw-logger.service pve-firewall.service pve-ha-lrm.service pve-ha-crm.service pvestatd.service pve-daily-update.timer pvenetcommit.service pvescheduler.service pve-storage.target
```

### clean all rrd data

```bash
rm -rf /var/lib/rrdcached/journal/rrd.* /var/lib/rrdcached/db/pve2-*/*
```

### clean all logs and tasks

```bash
rm -rf /var/log/pve-firewall.log.* /var/log/pveam.log.* /var/log/pveproxy/access.log.* ; find /var/log/pve/tasks -type f -exec rm "{}" \; ; > /var/log/pve-firewall.log ; > /var/log/pveam.log ; > /var/log/pveproxy/access.log
```

### rename proxmox node

```bash
# shutdown services

# change names
vi /etc/hosts
vi /etc/hostname
cp -p /etc/hostname /etc/mailname
vi /etc/postfix/main.cf

# move configurations
cd /etc/pve/nodes/
mkdir -p new_name/qemu-server
mv old_name/qemu-server/* new_name/qemu-server/
rm -rf old_name

# clean all rrd data

# reboot
```
