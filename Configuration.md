# Netdata Configuration

Configuration files are placed in `/etc/netdata`.

## Netdata Daemon

The daemon configuration file is read from `/etc/netdata/netdata.conf`.

In this file you can configure all aspects of netdata. Netdata provides configuration settings for plugins and charts found when started. You can find all these settings, with their default values, by accessing the file `/netdata.conf` using the URL of your netdata server. For example check the configuration file of [netdata.firehol.org](http://netdata.firehol.org/netdata.conf).

The configuration file has sections stated with `[section]`. There will be the following sections:

1. `[global]` for global netdata daemon options
2. `[plugins]` for controlling which plugins the netdata will use
3. `[plugin:NAME]` one such section for each plugin enabled
4. `[CHART_NAME]` once such section for each chart defined

### Global options

setting | default | info
:------:|:-------:|:----
hostname|auto-detected|The hostname of the computer running netdata.
history|3600|The number of entries the netdata daemon will by default keep in memory for each chart dimension. This setting can also be configured per chart. Check **[[Memory Requirements]]** for more information.
config directory|`/etc/netdata`|The directory configuration files are kept.
plugins directory|`/usr/libexec/netdata/plugins.d`|The directory plugin programs are kept.
web files directory|`/usr/share/netdata/web`|The directory the web static files are kept.
cache directory|`/var/cache/netdata`|The directory the memory database will be stored if and when netdata exits. Netdata will re-read the database when it will start again, to continue from the same point.
log directory|`/var/log/netdata`|The directory the log files are kept. Check **[[Log Files]]** for more information.
host access prefix|<empty>|This is used in docker environments where /proc, /sys, etc have to be accessed via another path. You may also have to set SYS_PTRACE capability on the docker for this work. Check https://github.com/firehol/netdata/issues/43.
	# debug flags = 0x00000000
	# memory deduplication (ksm) = yes
	# debug log = /opt/netdata/var/log/netdata/debug.log
	# error log = /opt/netdata/var/log/netdata/error.log
	# access log = /opt/netdata/var/log/netdata/access.log
	# memory mode = save
	# update every = 1
	# run as user = netdata
	# web files owner = netdata
	# http port listen backlog = 100
	# port = 19999
	# ip version = any
	# disconnect idle web clients after seconds = 60
	# enable web responses gzip compression = yes


## Netdata Plugins
