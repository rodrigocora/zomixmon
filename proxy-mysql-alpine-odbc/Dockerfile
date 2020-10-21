FROM alpine:3.12

LABEL org.opencontainers.image.title="Zabbix proxy (MySQL)" \
      org.opencontainers.image.authors="Alexey Pustovalov <alexey.pustovalov@zabbix.com>" \
      org.opencontainers.image.vendor="Zabbix LLC" \
      org.opencontainers.image.url="https://zabbix.com/" \
      org.opencontainers.image.description="Zabbix proxy with MySQL database support" \
      org.opencontainers.image.licenses="GPL v2.0"

STOPSIGNAL SIGTERM


#ORACLE INSTANT_CLIENT

ENV LD_LIBRARY_PATH=/lib
ENV TNS_ADMIN=/lib/network/admin/

RUN wget https://download.oracle.com/otn_software/linux/instantclient/instantclient-basiclite-linuxx64.zip && \
    wget https://download.oracle.com/otn_software/linux/instantclient/instantclient-odbc-linuxx64.zip && \
    unzip -q instantclient-basiclite-linuxx64.zip && \
    unzip -q instantclient-odbc-linuxx64.zip && \
    rm -f instantclient-basiclite-linuxx64.zip && \
    rm -f instantclient-odbc-linuxx64.zip && \
    cd $(ls -d instantclient*/|head -n 1) && \
    rm -f *jdbc* *occi* *mysql* *jar uidrvci genezi adrci && \
    mv * /lib && \
    ln -s /lib/libc.so.6 /lib/libresolv.so.2 && \
    ln -s /lib64/ld-linux-x86-64.so.2 /lib/ld-linux-x86-64.so.2 && \
    ln -s /usr/lib/libnsl.so.2 /lib/libnsl.so.1 && \
    apk add libaio && \
    apk add libaio libnsl libc6-compat

#ORACLE INSTANT_CLIENT


#MSSQL
RUN wget https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.apk && \
    apk add --allow-untrusted msodbcsql17_17.6.1.1-1_amd64.apk
#MSSQL


#PostgreSQL
RUN apk add --no-cache postgresql-dev
#PostgreSQL

#MySQL
RUN apk add --no-cache mariadb-dev
#MySQL

RUN set -eux && \
    addgroup -S -g 1995 zabbix && \
    adduser -S \
            -D -G zabbix -G root \
            -u 1997 \
            -h /var/lib/zabbix/ \
        zabbix && \
    mkdir -p /etc/zabbix && \
    mkdir -p /var/lib/zabbix && \
    mkdir -p /var/lib/zabbix/enc && \
    mkdir -p /var/lib/zabbix/mibs && \
    mkdir -p /var/lib/zabbix/modules && \
    mkdir -p /var/lib/zabbix/snmptraps && \
    mkdir -p /var/lib/zabbix/ssh_keys && \
    mkdir -p /var/lib/zabbix/ssl && \
    mkdir -p /var/lib/zabbix/ssl/certs && \
    mkdir -p /var/lib/zabbix/ssl/keys && \
    mkdir -p /var/lib/zabbix/ssl/ssl_ca && \
    mkdir -p /usr/lib/zabbix/externalscripts && \
    mkdir -p /usr/share/doc/zabbix-proxy-mysql && \
    apk add --clean-protected --no-cache \
            tini \
            bash \
            iputils \
            libcurl \
            libevent \
            libldap \
            libssh \
            libxml2 \
            mariadb-client \
            mariadb-connector-c \
            net-snmp-agent-libs \
            openipmi-libs \
            pcre \
            unixodbc \
            fping && \
    rm -rf /var/cache/apk/*

ARG MAJOR_VERSION=5.0
ARG ZBX_VERSION=${MAJOR_VERSION}.4
ARG ZBX_SOURCES=https://git.zabbix.com/scm/zbx/zabbix.git

ENV TERM=xterm ZBX_VERSION=${ZBX_VERSION} ZBX_SOURCES=${ZBX_SOURCES} \
    MIBDIRS=/usr/share/snmp/mibs:/var/lib/zabbix/mibs MIBS=+ALL

LABEL org.opencontainers.image.documentation="https://www.zabbix.com/documentation/${MAJOR_VERSION}/manual/installation/containers" \
      org.opencontainers.image.version="${ZBX_VERSION}" \
      org.opencontainers.image.source="${ZBX_SOURCES}"

RUN set -eux && \
    apk add --no-cache --virtual build-dependencies \
            autoconf \
            automake \
            coreutils \
            curl-dev \
            libevent-dev \
            g++ \
            git \
            make \
            libssh-dev \
            libxml2-dev \
            mysql-dev \
            net-snmp-dev \
            openipmi-dev \
            openldap-dev \
            pcre-dev \
            unixodbc-dev && \
    cd /tmp/ && \
    git clone ${ZBX_SOURCES} --branch ${ZBX_VERSION} --depth 1 --single-branch zabbix-${ZBX_VERSION} && \
    cd /tmp/zabbix-${ZBX_VERSION} && \
    zabbix_revision=`git rev-parse --short HEAD` && \
    sed -i "s/{ZABBIX_REVISION}/$zabbix_revision/g" include/version.h && \
    ./bootstrap.sh && \
    export CFLAGS="-fPIC -pie -Wl,-z,relro -Wl,-z,now" && \
    ./configure \
            --datadir=/usr/lib \
            --libdir=/usr/lib/zabbix \
            --sysconfdir=/etc/zabbix \
            --prefix=/usr \
            --enable-agent \
            --enable-proxy \
            --with-mysql \
            --with-ldap \
            --with-libcurl \
            --with-libxml2 \
            --with-net-snmp \
            --with-openipmi \
            --with-openssl \
            --with-ssh \
            --with-unixodbc \
            --enable-ipv6 \
            --silent && \
    make -j"$(nproc)" -s dbschema && \
    make -j"$(nproc)" -s && \
    cp src/zabbix_proxy/zabbix_proxy /usr/sbin/zabbix_proxy && \
    cp src/zabbix_get/zabbix_get /usr/bin/zabbix_get && \
    cp src/zabbix_sender/zabbix_sender /usr/bin/zabbix_sender && \
    cp conf/zabbix_proxy.conf /etc/zabbix/zabbix_proxy.conf && \
    cat database/mysql/schema.sql > database/mysql/create.sql && \
    gzip database/mysql/create.sql && \
    cp database/mysql/create.sql.gz /usr/share/doc/zabbix-proxy-mysql/ && \
    cd /tmp/ && \
    rm -rf /tmp/zabbix-${ZBX_VERSION}/ && \
    chown --quiet -R zabbix:root /etc/zabbix/ /var/lib/zabbix/ && \
    chgrp -R 0 /etc/zabbix/ /var/lib/zabbix/ && \
    chmod -R g=u /etc/zabbix/ /var/lib/zabbix/ && \
    apk del --purge --no-network \
            build-dependencies && \
    rm -rf /var/cache/apk/*

EXPOSE 10051/TCP

WORKDIR /var/lib/zabbix

VOLUME ["/var/lib/zabbix/snmptraps"]

COPY ["docker-entrypoint.sh", "/usr/bin/"]

ENTRYPOINT ["/sbin/tini", "--", "/usr/bin/docker-entrypoint.sh"]

USER 1997

CMD ["/usr/sbin/zabbix_proxy", "--foreground", "-c", "/etc/zabbix/zabbix_proxy.conf"]