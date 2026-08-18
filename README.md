# zabbix-authelia-prometheus

Zabbix 7 template for monitoring Authelia through its Prometheus metrics endpoint.

## Overview

This repository contains a Zabbix template that scrapes Authelia metrics from the local Zabbix agent using `web.page.get` and converts the Prometheus exposition format into Zabbix items, triggers, and graphs.

This approach avoids exposing the Authelia metrics port directly to the Zabbix server or proxy. The Zabbix agent runs on the same host as Authelia and fetches the local metrics endpoint, which keeps deployment simple and reduces network exposure.

## Included monitoring

The template includes coverage for:

- Authentication success/failure rates and latency
- Authorization request counts by HTTP status
- HTTP request volumes and latency by status code
- OIDC-related samples such as consent, JWKS, revocation, token, and userinfo flows
- Go runtime statistics and process health metrics
- Prometheus scrape health and file descriptor usage
- Built-in triggers, calculated items, dashboards, and graphs

## Prerequisites

- Zabbix 7.0
- A local Zabbix agent on the same host as Authelia
- Authelia with the Prometheus metrics endpoint enabled and reachable via HTTP on the agent host

## Import and configure

1. Import `authelia_by_prometheus_zabbix_7.yaml` into your Zabbix instance.
2. Install and enable the Zabbix agent on the Authelia host.
3. Link the `Authelia by Prometheus` template to that host.
4. Confirm the metrics endpoint is accessible from the local agent on `localhost`.
5. If needed, adjust the host macros below:

- `{$AUTHELIA.METRICS.HOST}`: default `localhost`
- `{$AUTHELIA.METRICS.PATH}`: default `metrics`
- `{$AUTHELIA.METRICS.PORT}`: default `9959`

The template fetches the endpoint using the equivalent of:

```text
http://{$AUTHELIA.METRICS.HOST}:{$AUTHELIA.METRICS.PORT}/{$AUTHELIA.METRICS.PATH}
```

The endpoint should respond with HTTP 200 and valid Prometheus exposition output.

## Example validation

From the Authelia host, verify the endpoint is reachable:

```bash
curl -fsSL http://localhost:9959/metrics | head
```

If the endpoint is not available or returns an unexpected response, fix Authelia metrics exposure before applying the template.

## Notes

- The template expects the metrics endpoint to be reachable from the local Zabbix agent, not directly from the Zabbix server or proxy.
- The default host macros are designed for a common Authelia setup, but they can be overridden for non-standard hostnames, ports, or paths.
- The template includes alerting for missing metrics, elevated authentication failure rates, and elevated latency conditions.

## Repository contents

- `authelia_by_prometheus_zabbix_7.yaml` — Zabbix template export
- `README.md` — usage and setup notes
