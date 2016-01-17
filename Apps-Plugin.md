# Per Application Statistics Plugin

Per application statistics are collected using netdata's `apps.plugin`.

This plugin walks through the entire `/proc` filesystem and aggregates statistics for applications of interest, defined in `/etc/netdata/apps_groups.conf` (the default is [here](https://github.com/firehol/netdata/blob/master/conf.d/apps_groups.conf)).

The plugin internally builds a process tree (much like `ps fax` does), and groups processes together (evaluating both child and parent processes) so that the result is always a chart with a predefined set of dimensions (of course, only application groups found running are reported).

## Limitations
The values gathered here are not 100% accurate. They only include values for the processes **running**.

If an application is spawning children continuously, which are terminated in just a few milliseconds (like shell scripts do), the values reported will be inaccurate. Linux does report the values for the exited childs of a process. However, these values are reported to the parent process only when the child exits. If these values, of the exited child processes, were also aggregated in the charts below, the charts would have been full of spikes, presenting unrealistic utilization for each process group. So, we decided to ignore these values and present only the utilization of the currently running processes.

## Configuration

The configuration file is `/etc/netdata/apps_groups.conf` (the default is [here](https://github.com/firehol/netdata/blob/master/conf.d/apps_groups.conf)).

The configuration file works accepts multiple lines, each having this format:

```txt
group: process1 process2 ...
```

Process names should be given as they appear when running `ps -e`. The program will actually match the process names in the `/proc/PID/status` file. So, to be sure the name is right for a process running with PID ` X `, do this:

```sh
cat /proc/X/status
```

The first line on the output is `Name: xxxxx`. This is the process name `apps.plugin` sees.

The order of the lines in the file is important only if you include the same process name to multiple groups.
