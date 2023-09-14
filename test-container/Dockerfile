# Use base Debian 11 (bullseye) image, see https://hub.docker.com/_/debian
FROM debian:bullseye 

# See Dockerfile support for GitHub Actions
# https://docs.github.com/en/actions/creating-actions/dockerfile-support-for-github-actions

# Install additional packages
ENV DEBIAN_FRONTEND=noninteractive
RUN sed -i -e "s/ main[[:space:]]*\$/ main contrib/" /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y tzdata apt-transport-https bzip2 rsync ca-certificates curl gnupg openssh-client git jq sudo zip unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install JDK
RUN apt-get update \
    && apt-get install -y openjdk-11-jdk openjdk-11-jre \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x | sudo bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Chromium & Chromedriver
RUN apt-get update \
    && apt-get install -y --no-install-recommends chromium chromium-driver \
    && ln -s /usr/bin/chromium /usr/bin/chrome \
    && ln -s /usr/bin/chromium /usr/bin/chromium-browser

# Install Firefox & Geckodriver
ARG firefox_ver=113.0
ARG geckodriver_ver=0.33.0
RUN apt-get update \
    # Install dependencies for Firefox
    && apt-get install -y --no-install-recommends --no-install-suggests `apt-cache depends firefox-esr | awk '/Depends:/{print$2}'` libasound2 libxt6 libxtst6 \
    # Download and install Firefox
    && curl -fL https://ftp.mozilla.org/pub/firefox/releases/${firefox_ver}/linux-x86_64/en-GB/firefox-${firefox_ver}.tar.bz2 | tar xjf - -C /opt \
    && ln -s /opt/firefox/firefox /usr/bin/firefox \
    && firefox --version \
    # Download and install geckodriver
    && curl -fL https://github.com/mozilla/geckodriver/releases/download/v${geckodriver_ver}/geckodriver-v${geckodriver_ver}-linux64.tar.gz | tar xzf - -C /usr/bin \
    && chmod +x /usr/bin/geckodriver \
    && geckodriver --version \
    && rm -rf /var/lib/apt/lists/* /tmp/*

# Install xvfb
RUN apt-get update \
    && apt-get install -y xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*