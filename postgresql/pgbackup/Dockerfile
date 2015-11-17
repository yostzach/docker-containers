FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r postgres && useradd -r -g postgres postgres

RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main' $PG_MAJOR > /etc/apt/sources.list.d/pgdg.list
ENV PG_MAJOR 9.4
ENV PG_VERSION 9.4.4-1.pgdg80+1

RUN apt-get update && \
	apt-get install -y postgresql-client-$PG_MAJOR && \
	rm -rf /var/lib/apt/lists/*

VOLUME /pgbackup/data/

ADD pg_backup_rotated.sh /pg_backup_rotated.sh

CMD ["/pg_backup_rotated.sh"]
