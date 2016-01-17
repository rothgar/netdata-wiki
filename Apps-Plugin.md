# Per Application Statistics Plugin

Per application statistics are collected using netdata's `apps.plugin`.

This plugin walks through the entire `/proc` filesystem and aggregates statistics for applications of interest, defined in `/etc/netdata/apps_groups.conf` (the default is [here](https://github.com/firehol/netdata/blob/master/conf.d/apps_groups.conf)).

The plugin internally builds a process tree (much like `ps fax` does), and groups processes together (evaluating both child and parent processes) so that the result is always a chart with a predefined set of dimensions (of course, only application groups found running are reported).

## Limitations
The values gathered here are not 100% accurate. They only include values for the processes **running**.

If an application is spawning children continuously, which are terminated in just a few milliseconds (like shell scripts do), the values reported will be inaccurate. Linux does report the values for the exited childs of a process. However, these values are reported to the parent process only when the child exits. If these values, of the exited child processes, were also aggregated in the charts below, the charts would have been full of spikes, presenting unrealistic utilization for each process group. So, we decided to ignore these values and present only the utilization of the currently running processes.
