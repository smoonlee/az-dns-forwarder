FROM alpine:3.23.1

RUN apk add --no-cache bind \
    && mkdir -p /var/cache/bind \
    && mkdir -p /var/run/named \
    && chown -R named:named /var/cache/bind /var/run/named /etc/bind

COPY named.conf /etc/bind/named.conf

USER named

EXPOSE 5353/udp 5353/tcp

ENTRYPOINT ["/usr/sbin/named", "-g", "-c", "/etc/bind/named.conf"]
