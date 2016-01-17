# Welcome to the netdata!

A live demo of netdata is available at **[netdata.firehol.org](http://netdata.firehol.org)**.

---

**Netdata** is a daemon that collects data in realtime (per second) and presents a web site to view and analyze them.
The presentation is also real-time and full of interactive charts that precisely render all collected values.

It has been designed to be installed **on every system**, without disrupting the applications running on it:

1. It will just use some spare CPU cycles (check **[[Performance]]**).
2. It will use the memory you want it have (check **[[Memory Requirements]]**).
3. Once started, it does not use any disk I/O, apart its logging (check **[[Log Files]]**).

You can use it to monitor all your systems and applications. It will run on Linux PCs, servers or embedded devices.

Out of the box, it comes with plugins that collect key system metrics and metrics of popular applications.

## Why another monitoring tool?

The key goal of **netdata** is to help you achieve **operational excellence**.

To achieve that, it focuses on real-time visualization of what is happening on your systems or applications **now** and in the **recent past**.

- **netdata** is not an NMS.
- **netdata** is not a database.
- **netdata** is not an alerting mechanism.

**netdata** tries to visualize the truth of **now**, in its greatest detail, so that you can get insights of what is happening **now** and what **just happened**, on your systems and applications.

Its key value is: **non disruptive, real-time monitoring and visualization, in the greatest possible detail**.

---

# Documentation

This wiki is the whole of it. Other than the wiki, currently there is the... source code.

You should at least walk through the pages of the wiki. They have a good overview of netdata, what it can do and how to use it.

---

# Support

If you need help, please use the github issues section.

---


# FAQ

## Is it ready?

Software is never ready. There is always something to improve.

**Netdata is stable**. We use it on production systems without any issues.

## Is it released?

No it is not. It could be though.

## Can I code too?

Of course! Fork the repo, adapt as you see fit and create pull requests.
If you want to discuss your plans, open a github issue to start the discussion.

## Is there a roadmap?

Well, we plan to do these:

1. Rename all the charts to follow the naming conventions of OpenTSDB (which will probably require a few changes to the plugins API and the default web dashboard).
2. Allow custom dashboards to be defined using JSON files, to avoid writing HTML
3. Allow per chart documentation to be stored in server side JSON files (outside the dashboards and the netdata daemon)
4. Allow creating dashboards from other dashboards (so that complex dashboards can be created)
5. Improve the memory database (possibly using the internal dedupper, LZ4 compression, disk archiving, mirroring it to third party databases, etc).
6. Create more plugins. A lot more plugins.
7. Allow internal plugins to be forked to external processes (this will protect the netdata daemon from plugin crashes, allow different security schemas for each plugin, etc).
8. Document everything (especially the dashboard and plugin support)
