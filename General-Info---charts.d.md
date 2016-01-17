# Charts.d

Charts.d is BASH script that allows the data collection using simple scripts.

It has been designed so that the actual script that will do data collection will be permanently in memory, collecting data with as little overheads as possible.

Charts.d looks for scripts in `/usr/libexec/netdata/charts.d`. The scripts should have the filename suffix: `.chart.sh`.

# A charts.d collector

A charts.d collector is a BASH script defining a few functions.

For a collector called `X`, the following criteria must be met:

1. The collector script must be called `X.chart.sh` and placed in `/usr/libexec/netdata/charts.d`.

2. If the collector script needs a configuration, it should be called `X.conf` and placed in `/etc/netdata`. The configuration file `X.conf` is also a BASH script itself.

3. All functions and global variables defined in the script and its configuration, must begin with `X_`.

4. The following functions must be defined:

   - `X_check()` - returns 0 or 1 depending on whether the collector is able to run or not (following the standard Linux command line return codes: 0 = OK, the collector can operate and 1 = FAILED, the collector cannot be used).

   - `X_create()` - creates the netdata charts, following the standard netdata plugin guides as described in **[[External Plugins]]** (commands `CHART` and `DIMENSION`). The return value does matter: 0 = OK, 1 = FAILED.

   - `X_update()` - collects the values for the defined charts, following the standard netdata plugin guides as described in **[[External Plugins]]** (commands `BEGIN`, `SET`, `END`). The return value also matters: 0 = OK, 1 = FAILED.

5. The following global variables are available to be set:
   - `X_update_every` - is the data collection frequency for the collector script, in seconds.

The collector script may use more functions or variables. But all of them must begin with `X_`.

The standard netdata plugin variables are also available (check **[[External Plugins]]**).

## X_check()

The purpose of the BASH function `X_check()` is to check is the configuration of the script is working. It should also be used for detecting configuration when possible.

For example, if your collector is about monitoring a local mysql database, the `X_check()` function may attempt to connect to a local mysql database to find out if it can read the values it needs. Keep in mind that configuration should override auto-detection.

`X_check()` is run only once for the lifetime of the collector.

## X_create()

The purpose of the BASH function `X_create()` is to create the charts and dimensions using the standard netdata plugin guides (**[[External Plugins]]**).

`X_create()` will be called just once and only after `X_check()` was successful.

