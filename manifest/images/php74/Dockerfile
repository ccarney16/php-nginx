FROM --platform=$TARGETOS/$TARGETARCH docker.io/library/almalinux:9-minimal

LABEL maintainer="Cameron Carney <ccarney@zyphi.co>"

# Allows the end user to customize what php modules and packages they want on buildtime
ENV PHP_VERSION=7.4
ENV PHP_MODULES=bcmath,cli,common,fpm,gd,gmp,intl,json,mbstring,mysqlnd,opcache,pdo,pecl-zip,process,soap,sodium,xml,zstd
ENV EXTRA_PACKAGES=""

# Create the Caddy group and user with ID 500
RUN microdnf install -y shadow-utils tzdata && \
    groupadd -r -g 500 www-user && \
    useradd -r -g www-user -u 500 -d /var/lib/www-user -s /sbin/nologin -c 'Web User' www-user && \
    install -d -m 0750 -o www-user -g www-user /var/lib/www-user

# Install any required dependencies for the container to operate
RUN rpm --install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm && \
    rpm --install https://rpms.remirepo.net/enterprise/remi-release-9.rpm && \
    microdnf module enable -y php:remi-${PHP_VERSION} && \
    eval microdnf install -y ca-certificates nginx php-{$PHP_MODULES} \
        python3 python3-pip tini ${EXTRA_PACKAGES} && \
    pip3 install yacron && \
    microdnf clean all && \
    pip3 cache purge

# Copy configuration to containers
COPY ./../common/entrypoint.sh /entrypoint.sh
COPY ./../common/etc /etc

# Export a persistent volume for www-user to store persistent data
VOLUME /var/lib/www-user

WORKDIR /var/www/html

# Let tini execute /entrypoint. This allows proper reaping of processes
USER www-user
ENTRYPOINT [ "/usr/bin/tini", "--", "/entrypoint.sh" ]
CMD [ "start-web" ]
