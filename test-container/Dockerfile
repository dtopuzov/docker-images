# Use base Debian 11 (bullseye) image, see https://hub.docker.com/_/debian
FROM debian:bullseye

# See Dockerfile support for GitHub Actions
# https://docs.github.com/en/actions/creating-actions/dockerfile-support-for-github-actions

# Install additional packages
ENV DEBIAN_FRONTEND=noninteractive
RUN sed -i -e "s/ main[[:space:]]*\$/ main contrib/" /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y tzdata apt-transport-https bzip2 rsync ca-certificates curl wget gnupg openssh-client git jq sudo zip unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install JDK
RUN apt-get update \
    && apt-get install -y openjdk-17-jdk openjdk-17-jre \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x | sudo bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Chrome & Chromedriver
ARG chrome_ver=117.0.5938.88
ARG chrome_driver_ver=117.0.5938.88
RUN apt-get update \
    && apt-get install -y unzip xvfb libxi6 libgconf-2-4 jq libjq1 libonig5 libxkbcommon0 libxss1 libglib2.0-0 libnss3 \
    libfontconfig1 libatk-bridge2.0-0 libatspi2.0-0 libgtk-3-0 libpango-1.0-0 libgdk-pixbuf2.0-0 libxcomposite1 \
    libxcursor1 libxdamage1 libxtst6 libappindicator3-1 libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libxfixes3 \
    libdbus-1-3 libexpat1 libgcc1 libnspr4 libgbm1 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxext6 \
    libxrandr2 libxrender1 gconf-service ca-certificates fonts-liberation libappindicator1 lsb-release xdg-utils \
    && wget -N https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/${chrome_ver}/linux64/chrome-linux64.zip -P ~/  \
    && unzip ~/chrome-linux64.zip -d ~/  \
    && mv ~/chrome-linux64 ~/chrome  \
    && ln -s ~/chrome/chrome /usr/bin/chrome \
    && chmod +x ~/chrome \
    && rm ~/chrome-linux64.zip \
    && wget -N https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/${chrome_driver_ver}/linux64/chromedriver-linux64.zip -P ~/ \
    && unzip ~/chromedriver-linux64.zip -d ~/ \
    && mv ~/chromedriver-linux64 ~/chromedriver \
    && mv ~/chromedriver/chromedriver /usr/bin/chromedriver \
    && chmod +x /usr/bin/chromedriver \
    && rm -rf ~/chromedriver-linux64.zip \
    && rm -rf ~/chromedriver

# Install Firefox & Geckodriver
ARG firefox_ver=117.0
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
