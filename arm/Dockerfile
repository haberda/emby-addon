FROM emby/embyserver_arm32v7:4.6.0.37

LABEL io.hass.version="4.6.0.37" io.hass.type="addon" io.hass.arch="arm"

RUN ["sed", "-i", "s|config|data|g", "/etc/services.d/emby-server/run"]

VOLUME ["/share", "/data", "/media"]
