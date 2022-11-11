ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base/amd64:8.1.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}
ARG BUILD_ARCH
ARG EMBY_RELEASE=4.7.9.0

# add needed nvidia environment variables for https://github.com/NVIDIA/nvidia-docker
ENV NVIDIA_DRIVER_CAPABILITIES="compute,video,utility"

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install packages
RUN \
 echo "**** install packages ****" && \
 apt-get update && \
 apt-get install -y --no-install-recommends \
	mesa-va-drivers \
	cpio \
	rpm2cpio \
	bash \
	cifs-utils

# Make folders
RUN \
  mkdir -p /app/emby && \
  mkdir -p /data/

# Install emby
RUN \
  echo "**** install emby ****" && \
  if [ -z ${EMBY_RELEASE+x} ]; then \
	EMBY_RELEASE=$(curl -s https://api.github.com/repos/MediaBrowser/Emby.Releases/releases/latest \
	| jq -r '. | .tag_name'); \
  fi && \
  if [ $BUILD_ARCH = "armv7" ]; then \
    curl -o /tmp/emby.rpm -L https://github.com/MediaBrowser/Emby.Releases/releases/download/${EMBY_RELEASE}/emby-server-rpm_${EMBY_RELEASE}_armv7hl.rpm; \
  elif [ $BUILD_ARCH = "aarch64" ]; then \
    curl -o /tmp/emby.rpm -L https://github.com/MediaBrowser/Emby.Releases/releases/download/${EMBY_RELEASE}/emby-server-rpm_${EMBY_RELEASE}_aarch64.rpm; \
  else \
    curl -o /tmp/emby.rpm -L https://github.com/MediaBrowser/Emby.Releases/releases/download/${EMBY_RELEASE}/emby-server-rpm_${EMBY_RELEASE}_x86_64.rpm; \
  fi
  
RUN cd /tmp && \
    rpm2cpio emby.rpm \
 	| cpio -i --make-directories && \
    mv -t \
	/app/emby/ \
	/tmp/opt/emby-server/system/* \
	/tmp/opt/emby-server/lib/* \
	/tmp/opt/emby-server/bin/ff* \
	/tmp/opt/emby-server/etc \
	/tmp/opt/emby-server/extra/lib/* && \
    cd /

RUN echo "**** cleanup ****" && \
    rm -rf \
	/tmp/* \
	/var/lib/apt/lists/* \
	/var/tmp/*

COPY root/ /

RUN chmod -R +x /etc/cont-init.d/

RUN chmod -R +x /etc/services.d/emby/

RUN useradd -u 911 -U -d /data -s /bin/false abc && \
    usermod -G users abc 

# Build arugments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

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
