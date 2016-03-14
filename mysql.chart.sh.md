Documentation is not written yet.

The shell script code is [here](https://github.com/firehol/netdata/blob/master/charts.d/mysql.chart.sh)

This collector support any number of mysql servers.
It collects many of its statistics and generates 20+ charts.

By default (un-configured) it will attempt to connect to a mysql server at localhost. If it succeeds it will proceed.

You can configure the mysql servers by editing `/etc/netdata/mysql.conf`.

To setup 2 servers (MY_A and MY_B) this is what you will need:

```sh

# you can define a different mysql client per server
# if you want to use the mysql from the system path,
# you can omit setting it.
mysql_cmds[MY_A]="/path/to/mysql"
mysql_cmds[MY_B]="/path/to/mysql"

# command line options to connect to the server
mysql_opts[MY_A]="-h serverA -u user"
mysql_opts[MY_B]="--defaults-file /etc/mysql/serverB.cnf"

```

Any number of servers can be configured.

Since this data collector is written in shell scripting, the connection to the mysql server is initiated and closed on each iteration.

It is in our TODOs to re-write it in node.js which will provide a more efficient way of collecting mysql performance counters.

