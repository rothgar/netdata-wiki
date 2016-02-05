# SNMP Data Collector

Using this collector, netdata can collect data from any SNMP device.

Any number of SNMP devices, with any number of charts having any number of dimensions can be defined. Each device may also have a different update frequency.

The source code of the plugin is [here](https://github.com/firehol/netdata/blob/master/node.d/snmp.node.js).

## Configuration

You will need to create the file `/etc/netdata/snmp.conf` with data like the following.

In this example:

 - the SNMP device is `10.11.12.8`.
 - the SNMP community is `public`.
 - we will update the values every 10 seconds (`update_every: 10` under the server `10.11.12.8`).
 - we define 2 charts `snmp_switch.bandwidth_port1` and `snmp_switch.bandwidth_port2`, each having 2 dimensions: `in` and `out`.

```js
{
	"enable_autodetect": false,
	"update_every": 5,
	"servers": [
		{
			"hostname": "10.11.12.8",
			"community": "public",
			"update_every": 10,
			"options": { "timeout": 10000 },
			"charts": {
				"snmp_switch.bandwidth_port1": {
					"title": "Switch Bandwidth for port 1",
					"units": "kilobits/s",
					"type": "area",
					"priority": 1,
					"dimensions": {
						"in": {
							"oid": "1.3.6.1.2.1.2.2.1.10.1",
							"algorithm": "incremental",
							"multiplier": 8,
							"divisor": 1024
						},
						"out": {
							"oid": "1.3.6.1.2.1.2.2.1.16.1",
							"algorithm": "incremental",
							"multiplier": -8,
							"divisor": 1024
						}
					}
				},
				"snmp_switch.bandwidth_port2": {
					"title": "Switch Bandwidth for port 2",
					"units": "kilobits/s",
					"type": "area",
					"priority": 1,
					"dimensions": {
						"in": {
							"oid": "1.3.6.1.2.1.2.2.1.10.2",
							"algorithm": "incremental",
							"multiplier": 8,
							"divisor": 1024
						},
						"out": {
							"oid": "1.3.6.1.2.1.2.2.1.16.2",
							"algorithm": "incremental",
							"multiplier": -8,
							"divisor": 1024
						}
					}
				}
			}
		}
	]
}
```

If you need to define many charts using incremental OIDs, you can use something like this:

This is like the previous, but the option `multiply_range` given, will multiply the current chart from `1` to `24` inclusive, producing 24 charts in total for the 24 ports of the switch `10.11.12.8`.

Each of the 24 new charts will have its id (1-24) appended at:

1. its chart unique id, i.e. `snmp_switch.bandwidth_port1` to `snmp_switch.bandwidth_port24`
2. its `title`, i.e. `Switch Bandwidth for port 1` to `Switch Bandwidth for port 24`
3. its `oid` (for all dimensions), i.e. dimension `in` will be `1.3.6.1.2.1.2.2.1.10.1` to `1.3.6.1.2.1.2.2.1.10.24`
3. its priority (which will be incremented for each chart so that the charts will appear on the dashboard in this order)

```js
{
	"enable_autodetect": false,
	"update_every": 10,
	"servers": [
		{
			"hostname": "10.11.12.8",
			"community": "public",
			"update_every": 10,
			"options": { "timeout": 20000 },
			"charts": {
				"snmp_switch.bandwidth_port": {
					"title": "Switch Bandwidth for port ",
					"units": "kilobits/s",
					"type": "area",
					"priority": 1,
					"multiply_range": [ 1, 24 ],
					"dimensions": {
						"in": {
							"oid": "1.3.6.1.2.1.2.2.1.10",
							"algorithm": "incremental",
							"multiplier": 8,
							"divisor": 1024
						},
						"out": {
							"oid": "1.3.6.1.2.1.2.2.1.16",
							"algorithm": "incremental",
							"multiplier": -8,
							"divisor": 1024
						}
					}
				}
			}
		}
	]
}
```

The `options` given for each server, are:

 - `timeout`, the time to wait for the SNMP device to respond. The default is 5000 ms.
 - `version`, the SNMP version to use. `0` is Version 1, `1` is Version 2c. The default is Version 1 (`0`).
 - `transport`, the default is `udp4`.
 - `port`, the port of the SNMP device to connect to. The default is `161`.
 - `retries`, the number of attempts to make to fetch the data. The default is `1`.


## Testing the configuration

To test it, you can run:

```sh
/usr/libexec/netdata/plugins.d/node.d.plugin 1 snmp debug
```

The above will run it on your console and you will be able to see very detailed information about each step. To have the output netdata needs omit the `debug` option.

## Speed

Keep in mind that many SNMP switches are routers are very slow. They many not be able to report values per second. If you run `node.d.plugin` in `debug` mode, it will report the time it took the SNMP device to respond. My switch, for example, needs 7-8 seconds to respond for the traffic on 24 ports (48 OIDs, in/out).

Also, if you use many SNMP clients on the same SNMP device at the same time, values may be skipped. This is a problem of the SNMP device, not this collector.

## Finding OIDs

Use `snmpwalk`, like this:

```sh
snmpwalk -t 20 -v 1 -O n -c public 10.11.12.8
```

- `-t 20` is the timeout in seconds
- `-v 1` is the SNMP version
- `-O n` will display OIDs in numeric format
- `-c public` is the SNMP community
- `10.11.12.8` is the SNMP device

Keep in mind that `snmpwalk` outputs the OIDs with a dot in front them. You should remove this dot when adding OIDs to the configuration file of this collector.

