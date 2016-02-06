# node.js plugins

These are the entities involved:

1. `node.d.plugin`

  `node.d.plugin` is a netdata plugin that provides an abstraction layer to allow you write netdata plugins in node.js quickly. It also manages all node.js plugins using a single instance of node, thus lowering the memory footprint of data collection.

2. Your data collection module should be split in 2 parts:

   - a `processor` to fetch the data from its source. `node.d.plugin` already provides an `http` collector to fetch data from web sources.
   - the chart generation, that will instruct netdata what charts to create and which values to update

3. Your module will automatically be able to process any number of servers, with different settings (even different data collection frequencies). You will write just the work needed for one, `node.d.plugin` will do the rest. For each server you are going to fetch data from, you will have to create a `service` (more later).

## writing a module

To provide a module called `mymodule`, you have create the file `/usr/libexec/netdata/node.d/mymodule.node.js`, with this structure:

```js

// this processor is needed only
// if you need a custom processor
// other than http
netdata.processors.myprocessor = {
	name: 'myprocessor',

	process: function(service, callback) {

		/* do data collection here */

		callback(data);
	}
};

// this is the mymodule definition
var mymodule = {
	processResponse: function(service, data) {

		/* generate charts and update values here */

	},

	configure: function(config) {
		var eligible_services = 0;

		if(typeof(config.servers) === 'undefined' || config.servers.length === 0) {

			/*
			 * create a service using internal default
			 * this is used for auto-detecting the settings
			 * if possible
			 */

			netdata.service({
				name: 'a name for this service',
				update_every: this.update_every,
				module: this,
				// any other information your processor needs
			}).execute(this.processResponse);

			eligible_services++;
		}
		else {

			/*
			 * create a service for each server in the
			 * configuration file
			 */

			var len = config.servers.length;
			while(len--) {
				var server = config.servers[len];

				if(typeof server.update_every === 'undefined')
					server.update_every = this.update_every;

				netdata.service({
					name: server.name,
					update_every: server.update_every,
					module: this,
					// any other information your processor needs
				}).execute(this.processResponse);

				eligible_services++;
			}
		}

		return eligible_services;
	},

	update: function(service, callback) {
		service.execute(function(service, data) {
			mymodule.processResponse(service, data);
			callback();
		});
	},
};

module.exports = mymodule;
```

### configure(config)

`configure(config)` is called just once, when `node.d.plugin` starts.
The config file will contain the contents of `/etc/netdata/mymodule.conf`.
This file should have the following format:

```js
{
	"enable_autodetect": false,
	"update_every": 5,
	"servers": [ { /* server 1 */ }, { /* server 2 */ } ]
}
```

If the config file `/etc/netdata/mymodule.conf` does not give a `enable_autodetect` or `update_every`, these will be added by `node.d.plugin`. So you module will always have them.

The configuration file `/etc/netdata/mymodule.conf` may contain whatever else is needed for `mymodule`.
