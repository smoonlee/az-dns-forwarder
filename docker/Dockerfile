FROM alpine:3.23.2

RUN apk add --no-cache bind \
    && mkdir -p /var/cache/bind \
    && mkdir -p /var/run/named

COPY named.conf /etc/bind/named.conf

EXPOSE 53/udp 53/tcp

ENTRYPOINT ["/usr/sbin/named", "-g", "-c", "/etc/bind/named.conf"]
