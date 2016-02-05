# Installation

## Automatic installation

Before you start, make sure you have `zlib` development files installed.

You also need to have a basic build environment in place. You will need packages like
`git`, `make`, `gcc`, `autoconf`, `autogen`, `automake`, `pgk-config`, etc.

To install them in debian/ubuntu, you need to run:

```sh
apt-get install zlib1g-dev gcc make git autoconf autogen automake pkg-config
```

Then do this to install and run netdata:

```sh

# download it
git clone https://github.com/firehol/netdata.git netdata.git
cd netdata.git

# build it
./netdata-installer.sh

```

The script `netdata-installer.sh` will build netdata and install it to your system. If you don't want to install it on the default directories, you can run the installer like this: `./netdata-installer.sh --install /opt`. This one will install netdata in `/opt/netdata`.

Once the installer completes, the file `/etc/netdata/netdata.conf` will be created.
You can edit this file to set options. To apply the changes you made, you have to restart netdata.

- You can start netdata by executing it with `/usr/sbin/netdata` (the installer will also start it).

- You can stop netdata by killing it with `killall netdata`.
    You can stop and start netdata at any point. Netdata saves on exit its round robbin
    database to `/var/cache/netdata` so that it will continue from where it stopped the last time.

To access the web site for all graphs, go to:

 ```
 http://127.0.0.1:19999/
 ```

You can get the running config file at any time, by accessing `http://127.0.0.1:19999/netdata.conf`.

To start it at boot, just run `/usr/sbin/netdata` from your `/etc/rc.local` or equivalent.

## node.js

I think the future of data collectors is node.js
So, please install `node`.

In old systems node.js is the package `nodejs`. In newer systems it is `node`.

If your system has `nodejs` but no `node`, you should:

```sh
ln -s $(which nodejs) /usr/bin/node
```

This command will link `nodejs` to `node`. Only if node.js is available as `node` the node.js collectors of netdata will work.

