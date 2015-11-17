nginx compiled with naxsi, ssl_module, ipv6, geoip_module and 4096-bit key for DHE
===================================================================================

(naxsi is an open-source module for nginx that add a DROP by-default firewall, blocking XSS and SQL injection attacks using learned rules)

nginx will be built from sources using Debian Jessie. For naxsi setup, follow https://github.com/nbs-system/naxsi/wiki/basicsetup

SSL details
------------

By default, DHE is using a 1024-bit key for the key-exchange, this image will generate a new 4096-bit length key

After image is built, tell nginx to use this new DHE for key-exchange, edit your ssl.conf:

.. code:: bash

    ssl_dhparam /etc/ssl/certs/dhparam.pem;

