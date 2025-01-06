FROM alpine:latest

RUN set -eux; \
    apk add --no-cache \
    curl \
    bash \
    shadow \
    musl \
    musl-locales-lang \
    musl-utils \
    musl-locales \
    tzdata \
    proftpd \
    proftpd-mod_lang \
    proftpd-mod_site_misc \
    proftpd-mod_rewrite \
    proftpd-mod_exec \
    proftpd-mod_sftp \
    proftpd-mod_unique_id \
    proftpd-mod_readme \
    proftpd-utils; \
    mkdir -p /var/run/proftpd/; \
    sed -i 's/ProFTPD Default Installation/ProFTPD/' /etc/proftpd/proftpd.conf; \
    sed -i 's/^MultilineRFC2228/#MultilineRFC2228/' /etc/proftpd/proftpd.conf; \
    sed -i '/^#MultilineRFC2228/a WtmpLog off' /etc/proftpd/proftpd.conf; \
    sed -i '/^Port/a PassivePorts 30000 30100' /etc/proftpd/proftpd.conf; \
    sed -i '/^DenyFilter/a MasqueradeAddress 127.0.0.1' /etc/proftpd/proftpd.conf; \
    sed -i '/^MasqueradeAddress/a SysLogLevel error' /etc/proftpd/proftpd.conf; \
    sed -i '/^SysLogLevel/a SystemLog /var/log/proftpd/proftpd.log' /etc/proftpd/proftpd.conf; \
    sed -i '/^SystemLog/a TransferLog /var/log/proftpd/xfer.log' /etc/proftpd/proftpd.conf;

ADD entrypoint /usr/bin/entrypoint
ADD processing /usr/local/bin/processing
ADD conf.d /etc/proftpd/conf.d
RUN chmod +x /usr/bin/entrypoint

EXPOSE 21
ENTRYPOINT ["entrypoint"]
