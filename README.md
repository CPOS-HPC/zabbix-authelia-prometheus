# zabbix-authelia-prometheus

Zabbix 7.0 template for monitoring Authelia through its Prometheus metrics endpoint.

## Setup

1. Import `authelia_by_prometheus_zabbix_7.yaml` into Zabbix.
2. Link the **Authelia by Prometheus** template to the monitored host.
3. Set the host-level macro `{$AUTHELIA.METRICS.URL}` to the Authelia metrics endpoint that Zabbix can reach, for example `http://authelia:9959/metrics`.

The template defaults this macro to `http://127.0.0.1:9959/metrics`. Override it on each host when Authelia runs elsewhere. The configured URL must return HTTP 200 and Prometheus exposition data.