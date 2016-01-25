# netdata via apache's mod_proxy

To pass netdata through mod_proxy, use a config like this:

```
<VirtualHost *:80>
        RewriteEngine On
        ProxyRequests Off
        <Proxy *>
                Order deny,allow
                Allow from all
        </Proxy>

        # first netdata server
        ProxyPass "/netdata/box/" "http://127.0.0.1:19999/" connectiontimeout=5 timeout=30
        ProxyPassReverse "/netdata/box/" "http://127.0.0.1:19999/"
        RewriteRule ^/netdata/box$ http://%{HTTP_HOST}/netdata/box/ [L,R=301]

        # second netdata server
        ProxyPass "/netdata/pi1/" "http://10.11.12.81:19999/" connectiontimeout=5 timeout=30
        ProxyPassReverse "/netdata/pi1/" "http://10.11.12.81:19999/"
        RewriteRule ^/netdata/pi1$ http://%{HTTP_HOST}/netdata/pi1/ [L,R=301]

        # rest of virtual host config
```

The `RewriteRule` statements make sure the request has a trailing `/`. Without a trailing slash, the browser will be requesting wrong URLs.

