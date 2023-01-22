# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/hassio-addons/base:13.0.0
FROM $BUILD_FROM

# See https://github.com/librespot-org/librespot/wiki/Compiling#addition-features for librespot compile features.
RUN \
	apk add --no-cache --virtual .build-dependencies \
        build-base=0.5-r3 \
        cargo=1.64.0-r2 \
        git=2.38.3-r1 \
        protobuf-dev=3.21.9-r0 \
        pulseaudio-dev=16.1-r6 \
        avahi-dev=0.8-r6 \
    \
    && apk add --no-cache \
        dbus=1.14.4-r0 \
        nss=3.85-r0 \
        avahi-compat-libdns_sd=0.8-r6 \
        avahi=0.8-r6 \
        pulseaudio=16.1-r6 \
        alsa-plugins-pulse=1.2.7.1-r0 \
		snapcast=0.26.0-r3 \
    \
    && cargo install \
        --locked \
        --no-default-features \
        --features with-dns-sd \
        --root /usr \
        --bin librespot \
        --version 0.4.2 \
        --verbose \
        -- librespot \
    \
    && apk del --no-cache --purge .build-dependencies \
    && rm -fr \
        /tmp/* \
        ~/.cargo \
        /usr/.crates.toml \
        /usr/.crates2.json

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="corneyl" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Corneyl's addons" \
    org.opencontainers.image.authors="corneyl" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}