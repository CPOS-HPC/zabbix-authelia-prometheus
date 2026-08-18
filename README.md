# zabbix-authelia-prometheus

Zabbix 7.0 template for monitoring Authelia through its Prometheus metrics endpoint.

## Setup

1. Import `authelia_by_prometheus_zabbix_7.yaml` into Zabbix.
2. Install and enable a local Zabbix agent on the same host running Authelia.
3. Link the **Authelia by Prometheus** template to that host.
4. Ensure the Authelia metrics endpoint is reachable from the local agent on `localhost` and configure the host macros if needed:
   - `{$AUTHELIA.METRICS.HOST}` defaults to `localhost`
   - `{$AUTHELIA.METRICS.PATH}` defaults to `metrics`
   - `{$AUTHELIA.METRICS.PORT}` defaults to `9959`

The template calls the Authelia `/metrics` endpoint through the local Zabbix agent using `web.page.get`, so the Zabbix server or proxy does not need direct access to the Authelia port. The configured endpoint must return HTTP 200 and valid Prometheus exposition data.