# Why another monitoring tool?

There are a lot of tools for collecting and visualizing performance metrics. Check for example [collectd](https://collectd.org/), [OpenTSDB](http://opentsdb.net/), [Cacti](http://www.cacti.net/) the many complete NMS systems that offer complete monitoring solutions ([Nagios](https://www.nagios.org/), [Zabbix](http://www.zabbix.com/), [OpenNMS](http://www.opennms.org/), etc.

So, why netdata?

Well, my need for netdata arose from the following facts:

1. Take any other performance monitoring solution. At the end of the day you will have to login to the server to understand what exactly is happening. You will have to use `iostat`, `iotop`, `vmstat`, `top`, `mytop` and probably a few dozen more console tools to figure it out. With netdata, this need is eliminated significantly. If you use netdata you will prefer it over the console tools. Netdata visualizes the data, while the console tools just show them. The detail is the same (actually I have spent pretty much time reading the code of the console tools to figure out what I need to do in netdata, so that the data will be the same).

2. Any performance monitoring solution that does not go down to per second collection and visualization of the data, I believe is useless. It will make you happy to have it, but it will not help you other than psychologically. Visualizing the present in real-time and in great detail, is the most important value a performance monitoring solution should provide. The next most important is the last hour. The next is the last 8 hours. The next is the last day and so on up to about the last 30 days (for me even this is too much). In my 20+ years in IT, I needed just once or twice to look a year back. And this was mainly out of curiosity.

3. Over-complexity. Most solutions require from you endless configuration of whatever imaginable. This is too much. I run a mysql server here. Why should I need to configure every single metric the database server provides? Show them all! We should be able to install something, do a simple configuration to enable monitoring of the application as a whole and start monitoring it immediately.

4. Most solutions require dedicated servers to actually use the monitoring console. This one really drives me mad. All of us have a spectacular tool on our desktops, that allows us to connect in real time to any server in the world: the web browser. It shouldn't be so hard to use the same tool to connect in real-time to all our servers! There is no need to centralize anything, to copy the data, aggregate them, or whatever. The data of the server is the data of the server. I don't need to copy the whole of opensource.com to view it. Why do I need to copy all my performance metrics to view them?

# Netdata is unique!

## Per second data collection and visualization

**Per second data collection and visualization** is usually only available in dedicated console tools, like `top`, `vmstat`, `iostat`, etc. Netdata brings per second data collection and visualization to all applications, accessible through the web.

*You are not convinced per second data collection is important?*
**Click** this image for a demo:

[![image](https://cloud.githubusercontent.com/assets/2662304/12373555/abd56f04-bc85-11e5-9fa1-10aa3a4b648b.png)](http://netdata.firehol.org/demo2.html)

## Realtime

When netdata runs on modern computers (even on CELERON processors), most chart queries are replied in less than 3 milliseconds! **Not seconds, MILLISECONDS!** Less than 3 milliseconds for calculating the chart, generating JSON text, compressing it and sending it to your web browser. Timings are logged in netdata's `access.log` for you to examine.

Netdata is written in plain `C` and the key system plugins are written in `C` too. Its speed can only be compared to the native console system administration tools.

You can also stress test your netdata installation by running the script `tests/stress.sh` found in the distribution. Most modern server hardware can serve more than 300 chart refreshes per second per core. A raspberry pi 2, can serve 300+ chart refreshes per second utilizing all of its 4 cores.

## No disk I/O at all

Netdata does not use any disk I/O, apart its logs and even these can be disabled.

Netdata will use some memory (you size it, check [[Memory Requirements]]) and CPU (below 2% of a single core for the daemon, plugins may require more, check [[Performance]]), but normally your systems should have plenty of these resources available and spare.

The design goal of **NO DISK I/O AT ALL** effectively means netdata will not disrupt your applications.

## No root access

You don't need to run netdata as root. If started as root, netdata will switch to the `netdata` user (or any other user given in its configuration or command line argument).

There are a few plugins that in order to collect values need root access. These (and only these) are setuid to root.

## Embedded web server

No need to run something else to access netdata. Of course you can use a firewall, or a reverse proxy, to limit access to it. But for most systems, inside your DMZ, just running it will be enough.

## Configuration-less

Netdata supports plenty of [[Configuration]]. Thought, we have done the most to allow netdata auto-detect most if not everything.

Even netdata plugins are designed to support configuration-less operation. So, you just install and run netdata. You will need to configure something only if it cannot be auto-detected.

## Visualizes QoS

Netdata visualizes `tc` QoS classes automatically. If you also use FireQOS, it will also collect interface and class names.

Check this animated GIF (generated with [ScreenToGif](https://screentogif.codeplex.com/)):

![animation5](https://cloud.githubusercontent.com/assets/2662304/12373715/0da509d8-bc8b-11e5-85cf-39d5234bf976.gif)

