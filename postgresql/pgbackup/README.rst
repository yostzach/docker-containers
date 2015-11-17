Backup PostgreSQL database using a container
============================================

When this container is started, it will generate a new backup, backups are going to be rotated, daily backups will be keep 7 days and weekly backup will be taken each 5 days and keep 5 weeks.

Container is built using Debian Jessie


Environmet variables for config
-------------------------------

For disk persistence, assign a volumen in `/pgbackup`

* POSTGRES_USER -> PostgreSQL user to do the backup
* POSTGRES_PASSWORD -> Password for the user
* POSTGRES_IP -> IP of the database server

Example (using etcd stored config, image is named *pgbackup* and container *pgbackup*

.. code::bash

    /usr/bin/docker run --name pgbackup -v /mnt/pgbackup:/pgbackup -e POSTGRES_PASSWORD=$(/usr/bin/etcdctl get /config/postgres_password) -e POSTGRES_USER=$(/usr/bin/etcdctl get /config/postgres_user) -e POSTGRES_IP=$(/usr/bin/etcdctl get /config/postgres_ip) pgbackup


