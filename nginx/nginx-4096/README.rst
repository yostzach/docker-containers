Stable nginx
=============

By default, DHE is using a 1024-bit key for the key-exchange, this image will generate a new 4096-bit length key

After image is built, tell nginx to use this new DHE for key-exchange, edit your ssl.conf:

.. code:: bash

    ssl_dhparam /etc/ssl/certs/dhparam.pem;

