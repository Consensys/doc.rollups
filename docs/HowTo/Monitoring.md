---
description: Monitor Rollups
---

# Monitor ConsenSys Rollups

You can use ConsenSys Rollups with [Prometheus](https://prometheus.io/) time-series database to record API usage metrics.
You can visualize the recorded data using an existing dashboard tool such as [Grafana](https://grafana.com/).

## Record API metrics

ConsenSys Rollups can record the following usage metrics for each request routed through its Handlers:

* Account creation
* Requests targeting an account
* Money order requests
* Outbound transfers
* Transfer requests received
* Transfer requests failed

Consensys Rollups also records metrics about the blockchain:

* Time to deserialize call data
* Time to deserialize inbound transfer data
* Time to deserialize outbound transfer data
* Time to validate a blockchain transaction batch
* Time to execute forced and standard transaction
* Time to fetch events from blockchain for the Connector

You can store these metrics in a Prometheus time-series database for further analysis.

Prometheus integrates well with the open source dashboard editor [Grafana](#grafana) to allow for easy
creation of dashboards to visualize the data being captured from ConsenSys Rollups.

### Prometheus

The [Prometheus documentation](https://prometheus.io/docs/introduction/overview/) provides information to set up
Prometheus to integrate with ConsenSys Rollups.
The [Prometheus first steps](https://prometheus.io/docs/introduction/first_steps/) is a good starting point.
A summary of the steps to store ConsenSys Rollups metrics in a Prometheus database is as follows:

1. Install Prometheus.
2. Create a `prometheus.yml` configuration file to to pull metrics from ConsenSys Rollups.
   For example, add the following YAML fragment to the `scrape_configs` block of the `prometheus.yml` file:

    !!! example "Example configuration"

        ```yml
        global:
          scrape_interval: 15s
        scrape_configs:
          - job_name: "prometheus"
            static_configs:
            - targets: ["localhost:9090"]
          - job_name: "rollups"
            scrape_timeout: 10s
            metrics_path: /metrics
            scheme: http
            static_configs:
            - targets: ["localhost:6012"]
        ```

3. Start Consensys Rollups.
4. Start Prometheus:

    ```bash
    prometheus --config.file=prometheus.yml
    ```

5. To view data stored in the database, access the Prometheus UI (by default `localhost:9090`, this address can be
   changed in `prometheus.yml`) and use the [Prometheus Query Language](https://prometheus.io/docs/prometheus/latest/querying/basics/).

### Grafana

Grafana can be used to create dashboards from data stored in Prometheus databases.
See the [Grafana documentation](https://grafana.com/docs) and [Grafana Getting Started](https://grafana.com/docs/guides/getting_strated) for details on how to set up a Grafana instance and integrate it with databases.
A summary of the steps is as follows:

1. [Install and start Grafana](https://grafana.com/docs/grafana/latest/installation) as described for your OS (if using the default configuration, Grafana will start on port 3000 and require login/password admin/admin to access the dashboard).
2. Create a data source to provide the necessary details to connect to the database.
3. Create a new dashboard.
4. Add panels to the dashboard. Panels are the graphs, tables, and statistics that make up a dashboard. The New Panel wizard allows the components of the panel to be configured:
   * Queries: Details the query to use retrieve data from the data source, see the following links for info on using the Query Editor for InfluxDB and Prometheus.
   * Visualization: How to present the data queried, including panel type, axis headings and more.
