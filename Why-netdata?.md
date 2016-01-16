# Netdata is unique!

1. Per second data collection.

   Per second data collection is usually only available in dedicated console tools, like `top`, `vmstat`, `iostat`, etc. Netdata brings per second data collection to all applications, available through the web.

   You are not convinced? Click this for a demo of why per second data collection is important:



2. Realtime

   When netdata runs on modern computers, all chart queries are replied in less than 1 millisecond! Yes, this is right 1 millisecond for calculating the chart, generating JSON text, compressing it and sending it to your web browser. Timings are logged in netdata `access.log` for you to examine.

3. No disk I/O

   Netdata does not use any disk I/O apart its logs and even these can be disabled.

   Netdata will use some memory (you size it) and CPU (below 2% of a single core for the daemon, plugins may require more), but normally your server systems should have enough of these resources.

   The design goal of **NO DISK I/O AT ALL** means netdata will not disrupt your applications.

4. Embedded web server

   No need to run something else to access netdata. Of course you can use a firewall to limit access to it, or 


