# NANO node monitoring stack

This docker-compose file deploys a NANO node monitoring stack consisting of the following services:

- `pushgateway`: An intermediary service which allows ephemeral and batch jobs to expose their metrics to Prometheus.
- `prom-exporter`: This is a custom scraper that collects data from NANO node instance.
- `prometheus`: The Prometheus monitoring system and time series database.
- `grafana`: A flexible and feature-rich metrics dashboard and graph editor.
- `grafana-renderer` (optional): A backend component that will render panels to PNG images.
- `pushgateway-cleaner`: A custom cleaner service that removes stale data from the pushgateway.

The stack comes with a default dashboard that can be found under `Dashboards > NANO > Overview`.

## Requirements

- Docker
- Docker-compose

## Running the Stack

To start the services, navigate to the directory containing the `docker-compose.yml` file and run:

```bash
docker compose up -d
```

This command will start the services in the background and leave them running.

If your node's RPC is running on a non-standard port, you can use the following command to override the default port to which the requests are made:

```bash
RPC_PORT=17076 docker compose up -d
```

## Accessing the Services

- `grafana`: Accessible at `http://localhost:3000` by default. The default credentials are `admin:admin`
- `pushgateway`: Accessible at `http://localhost:9091` by default.
- `prometheus`: Accessible at `http://localhost:9090` by default.

## Stopping the Stack

To stop the services, navigate to the directory containing the `docker-compose.yml` file and run:

```bash
docker compose down
```

## Configuration

Most of the configuration parameters are set using environment variables within the `.env` file. If you wish to change any service configuration, you can do so by editing this file.

The default values are as follows:

```
RPC_PORT=7076
NAME=nano-node
INTERVAL=3
PUSH_BIND=127.0.0.1
PUSH_PORT=9091
PROM_BIND=127.0.0.1
PROM_PORT=9090
PROM_DATA=prometheus_data
GRAFANA_BIND=127.0.0.1
GRAFANA_PORT=3000
GRAFANA_DATA=grafana_data
```