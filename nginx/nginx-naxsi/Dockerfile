FROM debian:jessie

MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"

ENV NGINX_VERSION 1.10.0
ENV NAXSI_VERSION 0.54

RUN gpg --keyserver pgp.mit.edu --recv-keys 2685AED4 A1C052F8 7BD9BF62

RUN apt-get update && \
    apt-get install -y build-essential gcc && \
    apt-get install -y --no-install-recommends wget ca-certificates && \
    apt-get install -y libpcre3 libpcre3-dev openssl libssl-dev geoip-bin libgeoip-dev && \
    rm -rf /var/lib/apt/lists/*

RUN wget -O /tmp/nginx.tar.gz http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
    wget -O /tmp/nginx.tar.gz.asc http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz.asc && \
    wget -O /tmp/naxsi.tar.gz https://github.com/nbs-system/naxsi/archive/${NAXSI_VERSION}.tar.gz && \
    wget -O /tmp/naxsi.tar.gz.asc https://github.com/nbs-system/naxsi/releases/download/${NAXSI_VERSION}/${NAXSI_VERSION}.tar.gz.asc && \
    apt-get purge -y --auto-remove wget

WORKDIR /tmp

RUN gpg --verify /tmp/nginx.tar.gz.asc /tmp/nginx.tar.gz && \
    gpg --verify /tmp/naxsi.tar.gz.asc /tmp/naxsi.tar.gz && \
    tar -xvzf /tmp/nginx.tar.gz && \
    tar -xvzf /tmp/naxsi.tar.gz && \
    cd nginx-${NGINX_VERSION} && ./configure --conf-path=/etc/nginx/nginx.conf --add-module=../naxsi-${NAXSI_VERSION}/naxsi_src/ \
	--error-log-path=/var/log/nginx/error.log --http-client-body-temp-path=/var/lib/nginx/body \
	--http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-log-path=/var/log/nginx/access.log \
	--http-proxy-temp-path=/var/lib/nginx/proxy --lock-path=/var/lock/nginx.lock \
	--pid-path=/var/run/nginx.pid --with-http_ssl_module \
	--without-mail_pop3_module --without-mail_smtp_module \
	--without-mail_imap_module --without-http_uwsgi_module \
	--without-http_scgi_module --with-ipv6 --with-http_geoip_module --prefix=/usr && \
    make && make install

# clean sources
RUN rm -rf /tmp/nginx* && rm -rf /tmp/naxsi*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

# improve security, use a 4096-bit key for the key-exchange
RUN cd /etc/ssl/certs && openssl dhparam -out dhparam.pem 4096

# create nginx user
RUN useradd -s /bin/false nginx && mkdir /var/lib/nginx

VOLUME ["/var/cache/nginx", "/repos"]

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
