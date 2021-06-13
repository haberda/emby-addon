ARG BUILD_FROM=emby/embyserver:4.6.2.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

RUN ["sed", "-i", "s|config|data|g", "/etc/services.d/emby-server/run"]

VOLUME ["/share", "/data", "/media"]

# Labels
LABEL \
    io.hass.name="Emby Media Server" \
    io.hass.description="Emby Media Server" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.version="${BUILD_VERSION}" \
    io.hass.type="addon" \
    maintainer="haberda" \
    org.label-schema.description="Emby Media Server" \
    org.label-schema.build-date="${BUILD_DATE}" \
    org.label-schema.name="Emby Media Server" \
    org.label-schema.schema-version="${BUILD_VERSION}" \
    org.label-schema.url="https://github.com/haberda/emby-addon" \
    org.label-schema.usage="https://github.com/haberda/emby-addon" \
    org.label-schema.vcs-ref="${BUILD_REF}" \
    org.label-schema.vcs-url="https://github.com/haberda/" \
    org.label-schema.vendor="No Vendor"