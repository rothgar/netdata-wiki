# Charts.d

Charts.d is BASH script that allows the data collection using simple scripts.

It has been designed so that the actual script that will do data collection will be permanently in memory, collecting data with as little overheads as possible.

Charts.d looks for scripts in `/usr/libexec/netdata/charts.d`. The scripts should have the filename suffix: `.chart.sh`.

# A charts.d collector

A charts.d collector is a BASH script defining a few functions. For a collector called `X`, the following criteria must be met:

1. The collector script must be called `X.chart.sh` and placed in `/usr/libexec/netdata/charts.d`.
2. If the collector script needs a configuration, it should be called `X.conf` and placed in `/etc/netdata`.
2. All functions and global variables defined in the script must begin with `X_`.
3. The following functions must be defined:
   - `X_check()` - returns 0 or 1 depending on whether the collector is able to run or not (following the standard Linux command line return codes: 0 = OK, the collector can operate and 1 = FAILED, the collector cannot be used).
   - `X_create()` - creates the netdata charts, following the standard netdata plugin guides as described in **[[External Plugins]]** (commands `CHART` and `DIMENSION`). The return value does matter: 0 = OK, 1 = FAILED.
   - `X_update()` - collects the values for the defined charts, following the standard netdata plugin guides as described in **[[External Plugins]]** (commands `BEGIN`, `SET`, `END`). The return value also matters: 0 = OK, 1 = FAILED.
4. The following global variables are available to be set:
   - `X_update_every` - is the data collection frequency for the collector script, in seconds.

The collector script may use more functions or variables. But all of them must begin with `X_`.

The standard netdata plugin variables are also available (check **[[External Plugins]]**).


