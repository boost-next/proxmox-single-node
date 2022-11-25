Installation influxDB: https://portal.influxdata.com/downloads/

Used Port: HTTP 8086

Installation Grafana: https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/

Used Port: HTTP 3000

Browse existing dashboards: https://grafana.com/grafana/dashboards/?dataSource=influxdb

How to monitor Proxmox with InfluxDB and Grafana: https://tcude.net/monitoring-proxmox-with-influxdb-and-grafana/

Use OpnSense with Telefgraf plugin: https://www.thomas-krenn.com/de/wiki/OPNsense_Telegraf_Plugin_Installation_und_Konfiguration


**Attention**

Create the setup of influxdb from the console via `influx setup` will store the `master admin token` directly into the user`s created folder `.influxdbv2/config`. The cli tool `influx` will be run directly afterwards with full access rights. Maybe do it as user `root` or create an operator user.

Doing the setup from the Web GUI, will not allow to show the `master admin token` anywhere. But root can use the `influxd` (!!! not `influx`) cli tool on InfluxDB server to read the token from installation directly out of the bolt database file.

Use:

```bash
influxd recovery auth list --bolt-path /var/lib/influxdb/influxd.bolt
```


## Appendix

Compare InfluxDB and Prometheus: https://blog.ordix.de/monitoring-vergleich-von-prometheus-und-influxdb#:~:text=Aufgrund%20des%20Pull%2DModells%20eignet,%2D%C3%9Cberwachunsdaten%20und%20IoT%2DSensoren.

