# Prometheus OVN Exporter Snap

This repository contains the [prometheus-ovn-exporter snap](https://snapcraft.io/prometheus-ovn-exporter) package of the [OVN Exporter](https://github.com/greenpau/ovn_exporter) for [Prometheus](https://prometheus.io).

## Installation

```commandline
sudo snap install prometheus-ovn-exporter
```

The snap requires privileged access to a number of system resources including the PID and unix sockets for the various OVN & OpenVSwitch processes which [cannot be granted auto-connection access](https://forum.snapcraft.io/t/allow-use-of-system-files-interface-and-auto-connection-for-prometheus-ovs-exporter-and-prometheus-ovn-exporter/32179/10).

The following commands are required to connect access to these various interfaces:

```commandline
snap connect prometheus-ovn-exporter:kernel-module-observe
snap connect prometheus-ovn-exporter:log-observe
snap connect prometheus-ovn-exporter:netlink-audit
snap connect prometheus-ovn-exporter:network-observe
snap connect prometheus-ovn-exporter:openvswitch
snap connect prometheus-ovn-exporter:run-openvswitch
snap connect prometheus-ovn-exporter:run-ovn
snap connect prometheus-ovn-exporter:system-observe
```

## Limitations

The upstream OVN Exporter currently assumes openvswitch (including ovs-vswitchd and ovsdb-server) will always be present on OVN nodes, however this is not true on the ovn-central units as part of a charmed deployment of OVN or OpenStack.

As a result, in such environments you will get errors about opening the openvswitch PID and database sockets. These can be ignored.

The ovn\_up metric will also always report 0 (Down) because these are not running.

Upstream Bug: <https://github.com/greenpau/ovn_exporter/issues/9>

## Configuration

The prometheus-ovn-exporter snap provides the following configuration variables that
can be set via the `snap set` command:

* __web.listenaddress__ - the IP:port to listen on, defaults to `:9476`

## Integration

The [ovn-central charm](https://charmhub.io/ovn-central) v22.09+ will automatically install and configure this snap.

v22.03 users can manually install it and configure the prometheus static-targets configuration option.

## License

This project is licensed under the terms of the Apache License 2.0. See the LICENSE file for details.
