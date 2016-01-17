# Access Point Plugin (ap)

The `ap` collector visualizes data related to access points.

It does the following:

1. Runs `iw dev` searching for interfaces that have `type AP`.

   From the same output it collects the SSIDs each AP supports by looking for lines `ssid NAME`.

2. For each interface found, it runs `iw INTERFACE station dump`.

   From the output is collects:

   - rx/tx bytes
   - rx/tx packets
   - tx retries
   - tx failed
   - signal strength
   - rx/tx bitrate
   - expected throughput

3. For each interface found, it create 6 charts:

   - Number of Connected clients
   - Bandwidth
   - Packets
   - Transmit Issues
   - Average Signal
   - Average Bitrate (including average expected throughput)

   The above are averages for all connected clients.

   Example:
   ![image](https://cloud.githubusercontent.com/assets/2662304/12377654/9f566e88-bd2d-11e5-855a-e0ba96b8fd98.png)

## Configuration

You can only set `ap_update_every=NUMBER` to `/etc/netdata/ap.conf`, to give the data collection frequency.
