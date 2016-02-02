# ISC Bind Statistics

Using this netdata collector, you can monitor one or more ISC Bind servers.

## How it works

The plugin will execute (from within node.js) the equivalent of:

```
curl "http://localhost:8888/json/v1"
```

Normally, this produces a very long output. Here is a sample of it:

```
{
  "json-stats-version":"1.0",
  "boot-time":"2016-01-31T08:20:48Z",
  "config-time":"2016-01-31T09:28:03Z",
  "current-time":"2016-02-02T21:35:38Z",
  "opcodes":{
    "QUERY":244263,
    "IQUERY":0,
    "STATUS":0,
    "RESERVED3":0,
    "NOTIFY":0,
    "UPDATE":3770,
    "RESERVED6":0,
    "RESERVED7":0,
    "RESERVED8":0,
    "RESERVED9":0,
    "RESERVED10":0,
    "RESERVED11":0,
    "RESERVED12":0,
    "RESERVED13":0,
    "RESERVED14":0,
    "RESERVED15":0
  },
  "qtypes":{
    "A":88094,
    "NS":852,
    "CNAME":1,
    "SOA":1,
    "PTR":115124,
    "MX":273,
    "TXT":198,
    "AAAA":38865,
    "SRV":850,
    "ANY":5
  },
  "nsstats":{
    "Requestv4":248034,
    "ReqEdns0":1245,
    "ReqTSIG":3770,
    "ReqTCP":57,
    "AuthQryRej":1438,
    "RecQryRej":122,
    "Response":242504,
    "TruncatedResp":44,
    "RespEDNS0":1245,
    "RespTSIG":3770,
    "QrySuccess":202211,
    "QryAuthAns":117833,
    "QryNoauthAns":119082,
    "QryNxrrset":32332,
    "QrySERVFAIL":258,
    "QryNXDOMAIN":2372,
    "QryRecursion":40019,
    "QryDuplicate":5530,
    "QryFailure":1560,
    "UpdateDone":2488,
    "UpdateFail":1282,
    "UpdateBadPrereq":1261,
    "QryUDP":242658,
    "QryTCP":45,
    "OtherOpt":100
  },
  "views":{
    "local":{
      "zones":[
        {
          "name":"0.IN-ADDR.ARPA",
          "class":"IN",
          "serial":0
        },
        {
          "name":"10.IN-ADDR.ARPA",
          "class":"IN",
          "serial":0
        },

...
```


From this output it collects:

- Global Received Requests by IP version (IPv4, IPv6)
- Global Successful Queries
- Current Recursive Clients
- Global Queries by IP Protocol (TCP, UDP)
- Global Queries Analysis
- Global Received Updates
- Global Query Failures
- Global Query Failures Analysis
- Other Global Server Statistics
- Global Incoming Requests by OpCode
- Global Incoming Requests by Query Type
- Global Socket Statistics
- Per View Statistics (the following set will be added for each bind view):
   - View, Resolver Active Queries
   - View, Resolver Statistics
   - View, Resolver Round Trip Timings
   - View, Requests by Query Type

## Configuration

The collector reads the configuration file `/etc/netdata/named.conf` with the following contents:

```js
{
	"enable_autodetect": true,
	"update_every": 5,
	"servers": [
		{
			"name": "bind1",
			"url": "http://127.0.0.1:8888/json/v1",
			"update_every": 1
		},
		{
			"name": "bind2",
			"url": "http://10.1.2.3:8888/json/v1",
			"update_every": 2
		}
	]
}
```

You can add any number of bind servers.

## Auto-detection

Auto-detection is controlled by `enable_autodetect` in the config file. The default is enabled, so that if the collector can connect to `http://127.0.0.1:8888/json/v1` to receive bind statistics, it will automatically enable it.

## Bind configuration

To use this plugin, you have to have bind v9.10+ with its statistics channel enabled and compiled to provide statistics as a `JSON` object.

For more information on how to get your bind installation ready, please refer to the [bind statistics channel developer comments](http://jpmens.net/2013/03/18/json-in-bind-9-s-statistics-server/) and to [bind documentation](https://ftp.isc.org/isc/bind/9.10.3/doc/arm/Bv9ARM.ch06.html#statistics).

Normally, you will need something like this in your `named.conf`:

```
statistics-channels {
        inet 127.0.0.1 port 8888 allow { 127.0.0.1; };
        inet ::1 port 8888 allow { ::1; };
};
```

(use the IPv4 or IPv6 line depending on what you are using, you can also use both)

## Example netdata charts

