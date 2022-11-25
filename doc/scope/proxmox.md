After installing proxmox following package can be purged.

But make sure to re-run service anabling afterwards

```bash
apt purge ifupdown
systemctl enable networking
# or does the same
apt install --reinstall ifupdown2
```
