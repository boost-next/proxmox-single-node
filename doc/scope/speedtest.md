files to download for test

https://speed.hetzner.de/ (hetzner self fast)
http://fra36-speedtest-1.tele2.net/ (with hetzner full 1GBit performance)
https://turnkeyinternet.net/speed-test/ (with hetzner slow)
https://proof.ovh.net/ (with hetzner slow)

test speed

```bash
curl -L -O <resource from above list>
```

automated speedtest script

```bash
cd /tmp

curl -L -O https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py

python3 speedtest.py --help
```
