# Welcome to netdata!

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

**netdata** tries to visualize the truth of **now**, in its **greatest detail**, so that you can get insights of what is happening **now** and what **just happened**, on your systems and applications.

Its key value is: **non disruptive, real-time monitoring and visualization, in the greatest possible detail**.

## How it works

You run a daemon on your linux: `netdata`. This daemon is written in C and is extremely lightweight.

**netdata**:

  - Spawns threads to collect all the data from all sources - it uses **[[Internal Plugins]]** and **[[External Plugins]]** for this.
  - Keeps track of the collected values in memory (no disk I/O at all, check **[[Memory Requirements]]**).
  - Is a standalone web server that serves its static files, for rendering its dashboards.
  - It provides a **[[REST API v1]]** for your browser to access the data.

If you install it on all your systems, each **netdata** will be standalone. There is no *central* netdata. Your web browser is the only entity that can *connect* all the netdata installations together. netdata dashboards can have charts from multiple netdata installations and these charts will still behave, on your browser, as if they were coming from the same netdata server!

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

Netdata is stable. We use it on production systems without any issues.

## Is it released?

Yeap!. Check the [releases page](https://github.com/firehol/netdata/releases).

## Can I code too?

Of course! Fork the repo, adapt as you see fit and create pull requests.
If you want to discuss your plans, open a github issue to start the discussion.

## Is there a roadmap?

Well, we plan to do these:

1. Allow the default dashboard to connect to multiple netdata servers:

 - add `connect` to connect to more netdata servers
 - add `search` to find charts and dimensions on all connected servers
 - add `compare` to create a dynamic dashboard using charts from all connected servers

2. Allow custom dashboards to be defined using JSON files, to avoid writing HTML. These JSON dashboards should also include any user documentation needed.

   We are already pretty close for this. The default dashboard is abstracted enough to support it. It still needs some work though.

3. Allow creating dashboards from other dashboards (so that complex dashboards can be created).

4. Improve the memory database (possibly using the internal deduper, compression, disk archiving, mirroring it to third party databases, etc).

5. Create more plugins. A lot more plugins.

6. Allow internal plugins to be forked to external processes (this will protect the netdata daemon from plugin crashes, allow different security schemas for each plugin, etc).

7. Document everything (this is a work in progress already).
