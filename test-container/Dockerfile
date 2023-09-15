# Use base Ubuntu 22.04 (jammy) image, see https://hub.docker.com/_/ubuntu
FROM ubuntu:22.04 

# See Dockerfile support for GitHub Actions
# https://docs.github.com/en/actions/creating-actions/dockerfile-support-for-github-actions

# Install additional packages
ENV DEBIAN_FRONTEND=noninteractive
RUN sed -i -e "s/ main[[:space:]]*\$/ main contrib/" /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y tzdata apt-transport-https bzip2 rsync ca-certificates curl gnupg openssh-client git jq sudo zip unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x | sudo bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Chromium & Chromedriver
ARG CHROME_VERSION=117.0.5938.62
RUN curl -s -o  /tmp/chrome-linux64.zip https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/$CHROME_VERSION/linux64/chrome-linux64.zip \
    && unzip /tmp/chrome-linux64.zip -d /opt \
    && ln -s /opt/chrome-linux64/chrome /usr/local/bin/chrome \
    && rm /tmp/chrome-linux64.zip

ARG CHROME_DRIVER_VERSION=117.0.5938.62
RUN curl -s -o /tmp/chromedriver_linux64.zip https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/$CHROME_DRIVER_VERSION/linux64/chromedriver-linux64.zip \
	&& unzip /tmp/chromedriver_linux64.zip -d /opt/selenium \
	&& rm /tmp/chromedriver_linux64.zip \
	&& mv /opt/selenium/chromedriver-linux64/chromedriver /opt/selenium/chromedriver  \
	&& chmod 755 /opt/selenium/chromedriver \
	&& ln -s /opt/selenium/chromedriver /usr/bin/chromedriver

# Install Firefox & Geckodriver
RUN apt-get update \
    && apt-get install -y firefox firefox-geckodriver \
    && firefox --version && geckodriver --version \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install xvfb
RUN apt-get update \
    && apt-get install -y xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*