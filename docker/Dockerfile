FROM alpine:3.23.2

RUN apk add --no-cache bind

COPY named.conf /etc/bind/named.conf

EXPOSE 53/udp

ENTRYPOINT [ "/usr/sbin/named", "-g", "-c", "/etc/bind/named.conf" ]
