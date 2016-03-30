# Installation

Before you start, make sure you have `zlib` development files installed.

You also need to have a basic build environment in place. You will need packages like
`git`, `make`, `gcc`, `autoconf`, `autogen`, `automake`, `pkg-config`, etc.

This is how to install them on different distributions:

#### Debian / Ubuntu
You need to run:

```sh
apt-get install zlib1g-dev gcc make git autoconf autogen automake pkg-config
```

#### Centos / Redhat
You need to run:

```sh
yum install zlib-devel gcc make git autoconf autogen automake pkgconfig
```

#### ArchLinux

You need to run:

```sh
pacman -S --needed base-devel libmnl libnetfilter_acct zlib
```

or run the following (and ignore the rest of this article):

```sh
wget https://aur.archlinux.org/cgit/aur.git/snapshot/netdata.tar.gz
tar xf netdata.tar.gz && cd netdata/
makepkg -sri
```

---

## Install netdata

Do this to install and run netdata:

```sh

# download it
git clone https://github.com/firehol/netdata.git netdata.git --depth=1
cd netdata.git

# build it
./netdata-installer.sh

```

The script `netdata-installer.sh` will build netdata and install it to your system.

If you don't want to install it on the default directories, you can run the installer like this: `./netdata-installer.sh --install /opt`. This one will install netdata in `/opt/netdata`.

Once the installer completes, the file `/etc/netdata/netdata.conf` will be created (if you changed the installation directory, the configuration will appear in that directory too).

You can edit this file to set options. One common option to tweak is `history`, which controls the size of the memory database netdata will use. By default is `3600` seconds (an hour of data at the charts) which makes netdata use about 10-15MB of RAM (depending on the number of charts detected on your system). Check **[[Memory Requirements]]**.

To apply the changes you made, you have to restart netdata.

---

## Starting netdata at boot

To start it at boot, just run `/usr/sbin/netdata` from your `/etc/rc.local` or equivalent.

You can also find systemd and openrc scripts in the **[system](https://github.com/firehol/netdata/tree/master/system)** directory.

---

## Working with netdata

- You can start netdata by executing it with `/usr/sbin/netdata` (the installer will also start it).

- You can stop netdata by killing it with `killall netdata`.
    You can stop and start netdata at any point. Netdata saves on exit its round robbin
    database to `/var/cache/netdata` so that it will continue from where it stopped the last time.

To access the web site for all graphs, go to:

 ```
 http://127.0.0.1:19999/
 ```

You can get the running config file at any time, by accessing `http://127.0.0.1:19999/netdata.conf`.

---

## node.js

I believe the future of data collectors is node.js (those of you that may complain, don't think "system monitoring" - netdata already does this with `C`. Think API or remote service, monitoring).

Currently node.js is needed for SNMP polling and `named` monitoring.

So, please also install `nodejs` or `node`.

netdata `node.d.plugin` will search for the node.js executable in the system path, using the following names, in this order:

1. nodejs (by running `command -v nodejs`)
2. node (by running `command -v node`)
3. js (by running `command -v js`)

Keep in mind that you need **node.js**. There are also other versions of server side javascript, like spidermonkey. Only **node.js** will work with netdata.
