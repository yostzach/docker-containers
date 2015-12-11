# Based on svendowideit/ambassador
# do
#   docker build -t jneight/ambassador_udp .
# then to run it (on the host that has the real backend on it)
#   docker run -t -i -link redis:redis -name redis_ambassador -p 6379:6379 jneight/ambassador_udp
# on the remote host, you can set up another ambassador
#    docker run -t -i -name redis_ambassador -expose 6379 -e REDIS_PORT_6379_UDP=udp://192.168.1.52:6379 svendowideit/ambassador_udp sh
# you can read more about this process at https://docs.docker.com/articles/ambassador_pattern_linking/

# use alpine because its a minimal image with a package manager.
# prettymuch all that is needed is a container that has a functioning env and socat (or equivalent)
FROM    alpine:3.2

RUN apk update && \
    apk add socat && \
rm -r /var/cache/

CMD env | grep _TCP= | sed 's/.*_PORT_\([0-9]*\)_UDP=udp:\/\/\(.*\):\(.*\)/socat -t 100000000 UDP4-LISTEN:\1,fork,reuseaddr UDP4:\2:\3 \& wait/' | sh