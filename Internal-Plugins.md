# Standard System Monitoring Plugins

Internally the following plugins have been implemented:

 - `/proc/net/dev` (all network interfaces for all their values)
 - `/proc/diskstats` (all disks for all their values)
 - `/proc/net/snmp` (total IPv4, TCP and UDP usage)
 - `/proc/net/netstat` (more IPv4 usage)
 - `/proc/net/stat/nf_conntrack` (connection tracking performance)
 - `/proc/net/ip_vs/stats` (IPVS connection statistics)
 - `/proc/stat` (CPU utilization)
 - `/proc/meminfo` (memory information)
 - `/proc/vmstat` (system performance)
 - `/proc/net/rpc/nfsd` (NFS server statistics for both v3 and v4 NFS servers)
 - `/proc/interrupts` (total and per core hardware interrupts)
 - `/proc/softirqs` (total and per core software interrupts)
 - `ksm` Kernel Same-Page Merging performance (several files under `/sys/kernel/mm/ksm`).
 - `tc` classes (QoS classes - [with FireQOS class names](http://firehol.org/tutorial/fireqos-new-user/)) - there is also a [shell helper](https://github.com/firehol/netdata/blob/master/plugins.d/tc-qos-helper.sh) for this (most parsing is done by the plugin in `C` code).
 - `netdata` (internal netdata resources utilization)

In case this page is left behind in updates, the source code for runs these internal plugins is [here](https://github.com/firehol/netdata/blob/master/src/plugin_proc.c)


## Netfilter Accounting

There is also a plugin that collects NFACCT statistics. This plugin is currently disabled because it requires root access. I have to move it to an external plugin.

You can build netdata with it to test it though. Just run `./configure` with the option `--enable-plugin-nfacct` (and any other options you may need). Remember, you have to tell netdata you want it to run as `root` for this plugin to work.