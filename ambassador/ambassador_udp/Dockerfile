# Based on svendowideit/ambassador
# do
#   docker build -t jneight/ambassador_udp .
# then to run it (on the host that has the real backend on it)
#   docker run -t -i --name graylog_ambassador --link graylog:graylog -p 12201:12201 jneight/ambassador_udp
# on the remote host, you can set up another ambassador
#    docker run -t -i --name graylog_ambassador --expose 12201/udp -e GRAYLOG_PORT_12201_UDP=udp://192.168.1.52:12201 jneight/ambassador_udp sh
# you can read more about this process at https://docs.docker.com/articles/ambassador_pattern_linking/

# use alpine because its a minimal image with a package manager.
# prettymuch all that is needed is a container that has a functioning env and socat (or equivalent)
FROM alpine:3.2

RUN apk update && \
    apk add socat && \
rm -r /var/cache/

ENV TIMEOUT 10

ENTRYPOINT env | grep _UDP= | sed 's/.*_PORT_\([0-9]*\)_UDP=udp:\/\/\(.*\):\(.*\)/socat -T $(echo $TIMEOUT) -t $(echo $TIMEOUT) UDP-RECVFROM:\1,fork,reuseaddr UDP-SENDTO:\2:\3 \& wait/' | sh
