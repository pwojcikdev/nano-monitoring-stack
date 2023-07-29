# NANO node monitoring stack

This docker-compose file deploys a NANO node monitoring stack consisting of the following services:

- `pushgateway`: An intermediary service which allows ephemeral and batch jobs to expose their metrics to Prometheus.
- `prom-exporter`: This is a custom scraper that collects data from NANO node instance.
- `prometheus`: The Prometheus monitoring system and time series database.
- `grafana`: A flexible and feature-rich metrics dashboard and graph editor.
- `pushgateway-cleaner`: (optional) A custom cleaner service that removes stale data from the pushgateway.
- `grafana-renderer` (optional, disabled by default): A backend component that will render panels to PNG images.

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

If your node's RPC is running on a non-standard port, or if your RPC is hosted on a different host, you can use the following command to override the default values:

```bash
RPC_PORT=17076 RPC_HOST=192.168.1.10 docker compose up -d
```

## Docker Desktop Consideration

If you are using Docker Desktop (mostly on macOS and Windows), the way it refers to the host is different compared to the regular Docker runtime on Linux-based systems. Docker Desktop runs inside a virtual machine, and hence, the `localhost` or `127.0.0.1` in Docker refers to the VM itself, not the host machine where the Docker Desktop is running.

So, if you're running your NANO node on your local machine and trying to monitor it from a Docker container, you'll have to use `host.docker.internal` instead of `localhost` or `127.0.0.1` as the `RPC_HOST`.

For example:

```bash
RPC_HOST=host.docker.internal docker compose up -d
```

Furthermore, when using Docker Desktop, the node RPC most likely needs to listen on the external IP. This means you'll need to set `address = "::ffff:0.0.0.0"` inside your `rpc-config.toml` file. This configuration allows the RPC server to listen for connections from any network interface on your system, not just the localhost.

⚠️ Warning: Making your node RPC listen on an external address can be potentially dangerous and expose your node to unwanted network traffic. This should only be done if you understand the implications and have proper security measures in place. For any production deployments, we highly recommend using Linux with proper network isolation.

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
RPC_HOST=127.0.0.1
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
Remember to replace `RPC_HOST` and `RPC_PORT` with your NANO node's actual IP address and port number.