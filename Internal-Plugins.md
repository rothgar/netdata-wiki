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
 - `tc` classes (QoS classes - [with FireQOS class names](http://firehol.org/tutorial/fireqos-new-user/)) - there is also a [shell helper](https://github.com/firehol/netdata/blob/master/plugins.d/tc-qos-helper.sh) for this (most parsing is done by the plugin in `C` code).
- `ksm` Kernel Same-Page Merging performance (several files under `/sys/kernel/mm/ksm`).
