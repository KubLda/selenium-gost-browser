FROM selenium/node-base:latest
ARG AUTHORS
LABEL authors=${AUTHORS}

USER root

# Install Chromium
ARG CHROMIUM_DEB_SITE="http://deb.debian.org/debian"
RUN echo "deb ${CHROMIUM_DEB_SITE}/ sid main" >/etc/apt/sources.list.d/debian.list \
  && wget -qO- https://ftp-master.debian.org/keys/archive-key-12.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/debian-archive-keyring.gpg \
  && wget -qO- https://ftp-master.debian.org/keys/archive-key-12-security.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/debian-archive-security-keyring.gpg \
  && for d in bin lib lib32 lib64 libo32 libx32 sbin; do dpkg-divert --package base-files --no-rename --remove /$d; done \
  && apt-get update -qqy \
  && apt install -y unzip \
  && mkdir -p /tmp/chromium 

RUN wget https://repo.yandex.ru/yandex-browser/deb/pool/main/y/yandex-browser-stable/yandex-browser-stable_26.6.1.1003-1_amd64.deb -O /tmp/chromium/chromium.deb \
  && wget https://github.com/yandex/YandexDriver/releases/download/v26.6.1-stable/yandexdriver-26.6.1.1005-linux.zip -O /tmp/chromium/yandex.zip  \
  && apt-get -yf install /tmp/chromium/chromium.deb \
  && unzip -o /tmp/chromium/yandex.zip -d /tmp/yandex/ \
  && ls /tmp/yandex/* \
  && mv /tmp/yandex/yandexdriver /usr/bin/chromedriver\
  && chmod +x /usr/bin/chromedriver \
  && rm -rf /tmp/chromium \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/* /etc/apt/sources.list.d/debian.list

#=================================
# Chromium Launch Script Wrapper
#=================================
RUN mv /usr/bin/yandex-browser-stable /usr/bin/chromium

COPY wrap_chromium_binary /opt/bin/wrap_chromium_binary
RUN /opt/bin/wrap_chromium_binary \
    && chromium --version

#============================================
# Chromium cleanup script and supervisord file
#============================================
COPY chrome-cleanup.sh /opt/bin/chrome-cleanup.sh
COPY chrome-cleanup.conf /etc/supervisor/conf.d/chrome-cleanup.conf
COPY fluxbox-menu-browser /etc/X11/fluxbox/

RUN mkdir -p /run/dbus && chmod 755 /run/dbus

USER ${SEL_UID}

#============================================
# Dumping Browser information for config
#============================================
RUN mkdir -p /opt/selenium/browsers/chrome \
    && echo "chrome" > /opt/selenium/browsers/chrome/name \
    && chromium --version | awk '{print $2}' > /opt/selenium/browsers/chrome/version \
    && echo '{"goog:chromeOptions": {"binary": "${SE_BROWSER_BINARY_LOCATION:-/usr/bin/chromium}"}}' > /opt/selenium/browsers/chrome/binary_location

ENV SE_OTEL_SERVICE_NAME="selenium-node-yandex" \
    SE_NODE_ENABLE_MANAGED_DOWNLOADS="true" \
    SE_CHROME_ARGS="--disable-gpu"
