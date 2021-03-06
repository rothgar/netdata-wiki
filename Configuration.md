# Netdata Configuration

Configuration files are placed in `/etc/netdata`.

## Netdata Daemon

The daemon configuration file is read from `/etc/netdata/netdata.conf`.

In this file you can configure all aspects of netdata. Netdata provides configuration settings for plugins and charts found when started. You can find all these settings, with their default values, by accessing the URL `https://netdata.server.hostname:19999/netdata.conf`. For example check the configuration file of [netdata.firehol.org](http://netdata.firehol.org/netdata.conf).

The configuration file has sections stated with `[section]`. There will be the following sections:

1. `[global]` for global netdata daemon options
2. `[plugins]` for controlling which plugins the netdata will use
3. `[plugin:NAME]` one such section for each plugin enabled
4. `[CHART_NAME]` once such section for each chart defined

The configuration file is a `name = value` dictionary. Netdata will not complain if you set options unknown to it. When you check the running configuration by accessing the URL `/netdata.conf` on your netdata server, netdata will add a comment on settings it does not currently use.

### [global] section options

setting | default | info
:------:|:-------:|:----
hostname|auto-detected|The hostname of the computer running netdata.
history|3600|The number of entries the netdata daemon will by default keep in memory for each chart dimension. This setting can also be configured per chart. Check **[[Memory Requirements]]** for more information.
config directory|`/etc/netdata`|The directory configuration files are kept.
plugins directory|`/usr/libexec/netdata/plugins.d`|The directory plugin programs are kept.
web files directory|`/usr/share/netdata/web`|The directory the web static files are kept.
cache directory|`/var/cache/netdata`|The directory the memory database will be stored if and when netdata exits. Netdata will re-read the database when it will start again, to continue from the same point.
log directory|`/var/log/netdata`|The directory the log files are kept. Check **[[Log Files]]** for more information.
host access prefix|*empty*|This is used in docker environments where /proc, /sys, etc have to be accessed via another path. You may also have to set SYS_PTRACE capability on the docker for this work. Check [issue 43](https://github.com/firehol/netdata/issues/43).
debug flags|0x00000000|Bitmap of debug options to enable. For more information check **[[Tracing Options]]**.
memory deduplication (ksm)|yes|When set to `yes`, netdata will offer its in-memory round robin database to kernel same page merging (KSM) for deduplication. For more information check **[[Memory Deduplication - Kernel Same Page Merging - KSM]]**
debug log|`/var/log/netdata/debug.log`|The filename to save debug information. This file will not be created is debugging is not enabled. You can also set it to `syslog` to send the debug messages to syslog, or `none` to disable this log. For more information check **[[Tracing Options]]**.
error log|`/var/log/netdata/error.log`|The filename to save error messages for netdata daemon and all plugins (`stderr` is sent here for all netdata programs, including the plugins). You can also set it to `syslog` to send the errors to syslog, or `none` to disable this log.
access log|`/var/log/netdata/access.log`|The filename to save the log of web clients accessing netdata charts. You can also set it to `syslog` to send the access log to syslog, or `none` to disable this log.
memory mode|save|When set to `save` netdata will save its round robin database on exit and load it on startup. When set to `map` the cache files will be updated in real time (check `man mmap` - do not set this on systems with heavy load or slow disks - the disks will continuously sync the in-memory database of netdata). When set to `ram` the round robin database will be temporary and it will be lost when netdata exits.
update every|1|The frequency in seconds, for data collection. For more information see **[[Performance]]**.
run as user|`netdata`|The user netdata will run as.
web files owner|`netdata`|The user that owns the web static files. Netdata will refuse to serve a file that is not owned by this user, even if it has read access to that file. If the user given is not found, netdata will only serve files owned by user given in `run as user`.
http port listen backlog|100|The port backlog. Check `man 2 listen`.
port|19999|The port to listen for web clients.
ip version|any|Can be `any` to attempt opening both IPv4 and IPv6 ports, `ipv4` to attempt only IPv4, `ipv6` to attempt only IPv6. If the port cannot be opened, netdata will refuse to run.
disconnect idle web clients after seconds|60|The time in seconds to disconnect web clients after being totally idle.
enable web responses gzip compression|yes|When set to `yes`, netdata web responses will be GZIP compressed, if the web client accepts such responses.

### [plugins] section options

In this section there will be a boolean (`yes`/`no`) option for each plugin. Additionally, there will be the following options:

setting | default | info
:------:|:-------:|:----
checks|no|This is a debugging plugin for the internal latency of netdata.
enable running new plugins|yes|When set to `yes`, netdata will enable plugins not configured specifically for them. Setting this to `no` will disable all plugins you have not set to `yes` explicitly.
check for new plugins every|60|The time in seconds to check for new plugins in the plugins directory. This allows having other applications dynamically creating plugins for netdata.

## Netdata Plugins

The configuration options for plugins appear in sections following the pattern `[plugin:NAME]`.

### Internal Plugins

Most internal plugins will provide additional options. Check **[[Internal Plugins]]** for more information.


### External Plugins

External plugins will have only 2 options at `netdata.conf`:

setting | default | info
:------:|:-------:|:----
update every|the value of `[global].update every` setting|The frequency in seconds the plugin should collect values. For more information check **[[Performance]]**.
command options|*empty*|Additional command line options to pass to the plugin. 

External plugins that need additional configuration may support a dedicated file in `/etc/netdata`. Check their documentation.
