FROM alpine:3.23.0

RUN apk add --no-cache bind \
    && mkdir -p /etc/bind /var/cache/bind /var/run/named \
    && chown -R named:named /etc/bind /var/cache/bind /var/run/named

COPY named.conf /etc/bind/named.conf

USER named

EXPOSE 53/udp 53/tcp

ENTRYPOINT ["/usr/sbin/named", "-f", "-g", "-u", "named"]
