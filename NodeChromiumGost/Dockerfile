ARG NAMESPACE=selenium
ARG VERSION=latest
ARG BASE=node-base
FROM ${NAMESPACE}/${BASE}:${VERSION}
ARG AUTHORS
LABEL authors=${AUTHORS}


USER root

# Install Chromium
ARG CHROMIUM_VERSION="146.0.7680.153"
ARG CHROMIUM_DEB_SITE="http://deb.debian.org/debian"
RUN echo "deb ${CHROMIUM_DEB_SITE}/ sid main" >/etc/apt/sources.list.d/debian.list \
  && wget -qO- https://ftp-master.debian.org/keys/archive-key-12.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/debian-archive-keyring.gpg \
  && wget -qO- https://ftp-master.debian.org/keys/archive-key-12-security.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/debian-archive-security-keyring.gpg \
  && for d in bin lib lib32 lib64 libo32 libx32 sbin; do dpkg-divert --package base-files --no-rename --remove /$d; done \
  && apt-get update -qqy \
  && apt install -y unzip \
  && mkdir -p /tmp/chromium \
  && wget -q https://github.com/deemru/Chromium-Gost/releases/download/${CHROMIUM_VERSION}/chromium-gost-${CHROMIUM_VERSION}-linux-amd64.deb -O /tmp/chromium/chromium.deb \
  && wget -q https://storage.googleapis.com/chrome-for-testing-public/${CHROMIUM_VERSION}/linux64/chromedriver-linux64.zip -O /tmp/chromium/chromium-driver.zip  \
  && apt-get -qqyf install /tmp/chromium/chromium.deb \
  && unzip -o /tmp/chromium/chromium-driver.zip -d /tmp/chromium/ \
  && mv /tmp/chromium/chromedriver-linux64/chromedriver /usr/bin/chromedriver \
  && chmod +x /usr/bin/chromedriver \
  && mv /usr/bin/chromium-gost-stable /usr/bin/chromium \
  && rm -rf /tmp/chromium \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/* /etc/apt/sources.list.d/debian.list

#=================================
# Chromium Launch Script Wrapper
#=================================
COPY wrap_chromium_binary /opt/bin/wrap_chromium_binary
RUN /opt/bin/wrap_chromium_binary \
    && chromium --version

#============================================
# Chromium cleanup script and supervisord file
#============================================
COPY chrome-cleanup.sh /opt/bin/chrome-cleanup.sh
COPY chrome-cleanup.conf /etc/supervisor/conf.d/chrome-cleanup.conf
COPY fluxbox-menu-browser /etc/X11/fluxbox/

USER ${SEL_UID}

#============================================
# Dumping Browser information for config
#============================================
RUN mkdir -p /opt/selenium/browsers/chrome \
    && echo "chrome" > /opt/selenium/browsers/chrome/name \
    && chromium --version | awk '{print $2}' > /opt/selenium/browsers/chrome/version \
    && echo '{"goog:chromeOptions": {"binary": "${SE_BROWSER_BINARY_LOCATION:-/usr/bin/chromium}"}}' > /opt/selenium/browsers/chrome/binary_location

ENV SE_OTEL_SERVICE_NAME="selenium-node-chrome" \
    SE_NODE_ENABLE_MANAGED_DOWNLOADS="true" \ 


