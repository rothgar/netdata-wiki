# Netdata Performance

Netdata performance is affected by:

**Data collection**
- the number of charts for which data are collected
- the number of plugins running
- the technology of the plugins (i.e. BASH plugins are slower than binary plugins)
- the frequency of data collection

**Web clients accessing the data**
- the duration of the charts in the dashboard
- the number of charts refreshes requested

---

## Netdata Daemon

For most server systems, with a few hundred charts and a few thousand dimensions, the netdata daemon, without any web clients accessing it, should not use more than 2% of a single core.

In embedded systems, if the netdata daemon is using a lot of CPU without any web clients accessing it, you should lower the data collection frequency. To set the data collection frequency, edit `/etc/netdata/netdata.conf` and set `update_every` to a higher number (this is the frequency in seconds data are collected for all charts: higher number of seconds = lower frequency, the default is 1 for per second data collection). You can also set this frequency per module or chart. Check the **[[Configuration]]** section.

## Plugins

If a plugin is using a lot of CPU, you should lower its update frequency, or if you wrote it, re-factor it to be more CPU efficient. Check **[[External Plugins]]** for more details on writing plugins.

## CPU consumption when web clients are accessing dashboards

Netdata is very efficient when servicing web clients. On most server platforms, netdata should be able to serve 300+ web client requests per second per core for random chart durations.

