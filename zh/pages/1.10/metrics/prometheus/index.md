---
layout: layout.pug
title: Sending DC/OS Metrics to Prometheus
menuWeight: 4.5
excerpt: ""
preview: true
enterprise: false
---
The Prometheus metrics plugin supports sending metrics from the DC/OS metrics service to a [Prometheus](https://prometheus.io/) server. You must install a plugin on each node in your cluster. This plugin works with DC/OS 1.9.4 and higher.

**Prerequisite:**

- DC/OS is [installed](/1.10/installing/oss/)

# Install the DC/OS Prometheus metrics plugin

For each node in your cluster, transfer your plugin binary and then add a systemd unit to manage the service. This unit differs slightly between master and agent nodes.

1. On every node in your cluster:
    
    1. Download the *latest* Prometheus plugin binary from the [releases](https://github.com/dcos/dcos-metrics/releases) page.
    2. Rename the plugin to `dcos-metrics-prometheus-plugin` and move to `/opt/mesosphere/bin`.
    3. Assign permissions to the plugin: `chmod 0755 /opt/mesosphere/bin/dcos-metrics-prometheus-plugin`.
    4. Download the [environment file](https://raw.githubusercontent.com/dcos/dcos-metrics/master/plugins/prometheus/systemd/dcos-metrics-prometheus.env) to `/opt/mesosphere/etc`. This file sets the port to 8088. (8088 was chosen because 8080 is the standard, but was already reserved for Marathon). To serve on a different port, edit the environment file and set `PROMETHEUS_PORT`.
    5. Set the environment variable `DCOS_METRICS_CONFIG_PATH` to `/opt/mesosphere/etc`.

2. On every master node:
    
    1. Download the master [systemd service file](https://raw.githubusercontent.com/dcos/dcos-metrics/master/plugins/prometheus/systemd/dcos-metrics-prometheus-master.service) to `/etc/systemd/system`.
    2. Reload the systemd state by running `sudo systemctl daemon-reload`.
    3. Start the systemd service with `sudo systemctl start dcos-metrics-prometheus-master`.
    4. View the system logs and verify the plugin is running with `sudo journalctl -u dcos-metrics-prometheus-plugin`.

3. On every master node:
    
    1. Copy the agent [systemd service file](https://raw.githubusercontent.com/dcos/dcos-metrics/master/plugins/prometheus/systemd/dcos-metrics-prometheus-agent.service) to `/etc/systemd/system`.
    2. Reload the systemd state by running `sudo systemctl daemon-reload`.
    3. Start the systemd service with `sudo systemctl start dcos-metrics-prometheus-agent`.
    4. View the system logs and verify the plugin is running with `sudo journalctl -u dcos-metrics-prometheus-plugin`.

# Run a Prometheus server

1. Download a [Prometheus Marathon app definition](https://raw.githubusercontent.com/dcos/dcos-metrics/master/plugins/prometheus/marathon/prometheus.json).
2. Create a `prometheus.yml` configuration file. 
3. Configure the IP address of each node and the Prometheus port configured in the environment file in the `static_configs` array in the [`scrape_configs`](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#<scrape_config>) section.
4. Run the app with `dcos marathon app add prometheus.json`.

# Test a metric emitter

1. Download the [ statsd-emitter](https://raw.githubusercontent.com/dcos/dcos-metrics/master/plugins/prometheus/marathon/statsd-emitter.json) test task.
2. Run the app with `dcos marathon app add statsd-emitter.json`.
3. Check your Prometheus frontend for the `statsd_tester_time_uptime` metric. For example:
    
    ![statsd_tester_time_uptime](/1.10/img/statsd_tester_time_uptime.png)