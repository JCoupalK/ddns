FROM debian:bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl ca-certificates gnupg \
    && install -d /etc/apt/keyrings \
    && curl -fsSL https://repo.powerdns.com/CBC8B383-pub.asc | gpg --dearmor -o /etc/apt/keyrings/powerdns.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/powerdns.gpg] http://repo.powerdns.com/debian bookworm-auth-49 main" > /etc/apt/sources.list.d/pdns.list \
    && printf "Package: pdns-*\nPin: origin repo.powerdns.com\nPin-Priority: 600\n" > /etc/apt/preferences.d/pdns \
    && apt-get update \
    && apt-get install -y --no-install-recommends pdns-server pdns-backend-remote \
    && apt-get purge -y curl gnupg \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/*

COPY docker/powerdns/pdns.conf /etc/powerdns/pdns.conf
COPY docker/powerdns/entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 53/udp 53/tcp
CMD ["pdns_server", "--daemon=no"]
