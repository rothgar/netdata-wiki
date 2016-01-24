# Introducing netdata

Linux, non disruptive, real-time monitoring and visualization, in the greatest possible detail.

[Click here for a live demo](http://netdata.firehol.org)

An animated GIF showing netdata ability to monitor QoS:

![animation5](https://cloud.githubusercontent.com/assets/2662304/12373715/0da509d8-bc8b-11e5-85cf-39d5234bf976.gif)


### Why another monitoring tool?

There are a lot of tools for collecting and visualizing performance metrics. Check for example [collectd](https://collectd.org/), [OpenTSDB](http://opentsdb.net/), [Cacti](http://www.cacti.net/), the many complete NMS systems that offer full monitoring solutions ([Nagios](https://www.nagios.org/), [Zabbix](http://www.zabbix.com/), [OpenNMS](http://www.opennms.org/), etc.

So, why **netdata**?

### The end of console for performance monitoring

> Take any performance monitoring solution and try to troubleshoot a performance problem. At the end of the day you will have to login to the server to understand what exactly is happening. You will have to use `iostat`, `iotop`, `vmstat`, `top`, `iperf` and probably a few dozen more console tools to figure it out.

With **netdata**, this need is eliminated significantly. Of course you will login on the console. Just not for  monitoring performance!

If you install **netdata** you will prefer it over the console tools. **Netdata** visualizes the data, while the console tools just show their values. The detail is the same - actually I have spent pretty much time reading the source code of the console tools, to figure out what I need to do in netdata, so that the data will be the same. **Netdata** visualizes these data in ways you cannot even imagine on the console. It allows you to see the present in real-time, much like the console tools, but also the recent past, compare different metrics with each other (past and present), zoom in to see the recent past in detail, or zoom out to have a helicopter view of what is happening in longer durations.

### Real-time

> Any performance monitoring solution that does not go down to per second collection and visualization of the data, is useless. It will make you happy to have it, but it will not help you more than that. 

Visualizing the present in real-time and in great detail, is the most important value a performance monitoring solution should provide. The next most important is the last hour, again per second. The next is the last 8 hours. The next is the last day and so on, up to a week, or at most a month. In my 20+ years in IT, I needed just once or twice to look a year back. And this was mainly out of curiosity.

Of course real-time monitoring requires resources. This is why **netdata** is designed to be very efficient:

1. collecting performance data is a repeating process - you do the same thing again and again. **Netdata** has been designed to learn from each loop, so that the next one will be faster. It learns the sizes of files (it even keeps them open when it can), the number of lines and words per line they contain, the sizes of the buffers it needs to process them, etc. It adapts, so that everything will be as ready as possible for the next iteration.
2. internally, it uses indexes (b-trees), much like databases do, to speed up lookups of metrics, charts, dimensions, settings.
3. it has a in-memory round robin database based on a custom floating point number that allows it to pack values and flags together, very efficiently, to lower its memory footprint.
4. its internal web server is capable of generating JSON responses from live performance data with speeds comparable to static content delivery.

**Netdata** will use some CPU and memory, but it **will not produce any disk I/O at all**, apart its logs (which you can disable if you like).

Most servers should have plenty of CPU resources (I consider a hardware upgrade or application split when a server reaches constantly about 30-40% average CPU utilization). Even if a server has limited CPU resources available, you can just lower the data collection frequency of **netdata**. Going from per second to every 2 seconds, will cut the **netdata** CPU (and memory) requirements in half and you will still get charts that are just 2 seconds behind!

The same goes for memory. If you just keep an hour of data (which is pretty good for basic performance monitoring), you will most probably need 15-20MB. You can also enable the kernel de-duper (Kernel Same-Page Merging) and **netdata** will offer to it all its round robin database. KSM can free 20-60% of the memory used by **netdata** (guess why: there are a lot of metrics that are always zero or just constant).

### Simplicity

> Most solutions require endless configuration of whatever imaginable. This is too much. This is a linux box. Why do I need to configure every single metric I need to monitor. Guess what, the server has a CPU and some RAM and a few disks, and ethernet ports, it might run a firewall, a web server, or a database server and so on. Why do we need to configure all these metrics? Show them all!

**Netdata** has been designed to auto-detect everything. Of course you can enable, tweak or disable things. But by default, if **netdata** can connect to a mysql server you run on your linux box, it will automatically collect all performance metrics. The same is true for squid, apache, nginx, etc. It will also automatically collect all available system values for CPU, memory, disks, network interfaces, QoS, etc. Even for applications that do not offer performance metrics, it will automatically group the whole process tree and provide metrics like CPU usage, memory allocated, opened files, sockets, disk activity, swap activity, etc per application group.

### Monitoring console

> Most solutions require dedicated servers to actually use the monitoring console. Well, I think this is totally wrong! All of us have a spectacular tool on our desktops, that allows us to connect in real time to any server in the world: **the web browser**. It shouldn't be so hard to use the same tool to connect in real-time to all our servers...

With **netdata**, there is no need to centralize anything, no need to copy the data or aggregate them somewhere. The data of the server are data on the server. You view them directly from their source.

Of course, with **netdata** you can build dashboards with charts from any number of servers. And these charts will be connected to each other much like the ones that come from the same server. You will hover on one and all of them will show the relative value for the selected timestamp. You will zoom or pan one and all of them will follow. **Netdata** achieves that because the logic that connects the charts together is at your browser, not the server, so that all charts presented on the same page are connected, no matter where they come from.

The charting libraries **netdata** uses, are the fastest possible. **Netdata** sacrifices bandwidth for lower CPU resources. So to just view a live page full of charts that are updated per second, you will need 100 to 200kbps between your browser and the **netdata** server, but your browser will not have to do much. Data are just going to be rendered on a canvas. No processing at all. To achieve this, the netdata internal web server has been designed to respond in less than 3 milliseconds for most charts refreshes (many are below 1ms). Less than 3 milliseconds for parsing the request, calculating the response from the collected data, generating output in the format requested for each charting library, compressing it with gzip and sending it to your browser.

Server side, chart data generation scales pretty well. You can expect 400+ chart refreshes per second per core on modern hardware. For a page with 8 charts visible (the page may have hundreds, but only the visible are refreshed), just a tiny fraction of a single CPU core will be used for servicing them. Even these refreshes stop when you switch tabs on your browser, you focus on another window, scroll to a part of the page without charts, zoom or pan a chart. And of course the **netdata** server runs with the lowest possible process priority, so that your production environment, your applications will not be bothered by the netdata server.

## Is it ready?

**Netdata** is stable and usable.

There are more things we are willing to add to it, like these:

- [Custom dashboards](https://github.com/firehol/netdata/wiki/Custom-Dashboards) are now written in HTML (no javascript is required for this, but still you need to know HTML). It would be nice if custom dashboards could be created from the netdata UI itself.
- The default dashboard should allow us to search for charts, even on multiple servers.
- [Documentation](https://github.com/firehol/netdata/wiki) is still a work in progress.
- Netdata supports data collection [plugins in any language](https://github.com/firehol/netdata/wiki/External-Plugins), including shell scripting. Shell scripting is inefficient for per second updates, but it is excellent if you want to visualize something immediately and you don't care about the overheads. Netdata shell plugin (called `charts.d`) uses several techniques to eliminate these overheads, but still shell plugins shipped with **netdata** should be re-written in a more efficient language (personally, I like a lot `node.js` - it is perfect for async operations, fast enough for repeating operations where the program is already loaded, it will eliminate a lot of installation dependencies and `node.js` programs are just javascript source files easy to edit and adapt).
