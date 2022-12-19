Install missing packages

```bash
apt update
apt dist-upgrade
apt install curl zip unzip net-tools gnupg2 ifupdown2 screen
apt purge ifupdown
dpkg-reconfigure ifupdown2
apt autoremove --purge
```

system needs reboot afterwards

```bash
reboot
```
