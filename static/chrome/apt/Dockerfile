FROM browsers/base:7.4.2

ARG VERSION=noop
ARG PACKAGE=google-chrome-stable
ARG INSTALL_DIR=chrome

LABEL browser=$PACKAGE:$VERSION
LABEL maintainer="jeremieb"

# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

RUN \ 
    # Install required packages
        apt-get update && \
        apt-get -y --no-install-recommends install \
        gconf-service \
        libasound2 \
        libatk1.0-0 \
        libc6 \
        libcairo2 \
        libcups2 \
        libdbus-1-3 \
        libexpat1 \
        libfontconfig1 \
        libfreetype6 \
        libgcc1 \
        libgconf-2-4 \
        libgdk-pixbuf2.0-0 \
        libglib2.0-0 \
        libgtk2.0-0 \
        libgtk-3-0 \
        libnspr4 \
        libnss3 \
        libpango1.0-0 \
        libstdc++6 \
        libx11-6 \
        libx11-xcb1 \
        libxcb1 \
        libxcomposite1 \
        libxcursor1 \
        libxdamage1 \
        libxext6 \
        libxfixes3 \
        libxi6 \
        libxrandr2 \
        libxrender1 \
        libxss1 \
        libxtst6 \
        libgbm1 \
        libu2f-udev \
        libvulkan1 \
        ca-certificates \
        fonts-liberation \
        libappindicator3-1 \
        libnss3 \
        lsb-base \
        xdg-utils \
        libcurl4 \
        iproute2 \
        curl && \
    # Download selenium manager
        curl -fSLo /tmp/selenium-manager-linux https://github.com/SeleniumHQ/selenium_manager_artifacts/releases/download/selenium-manager-372cb5d/selenium-manager-linux && \
        chmod +x /tmp/selenium-manager-linux && \
        output=$(/tmp/selenium-manager-linux --browser chrome --browser-version $VERSION --avoid-stats --output JSON) && \
        browser_path=$(echo "$output" | jq -r '.result.browser_path') && \
        browser_path=${browser_path%/*} && \
        mkdir -p /opt/google/chrome/ && \
        mv ${browser_path}/* /opt/google/chrome && \
        rm /tmp/selenium-manager-linux && \
    # Fix default argument
        sed -i -e 's@exec -a "$0" "$HERE/chrome"@& --no-sandbox --disable-gpu@' \
        /opt/google/${INSTALL_DIR}/chrome || true && \
    # Fix permission
        if [ -f /opt/google/${INSTALL_DIR}/chrome_sandbox ]; then \
            chown root:root /opt/google/${INSTALL_DIR}/chrome_sandbox && \
            chmod 4755 /opt/google/${INSTALL_DIR}/chrome_sandbox; \
        fi && \
    # Symbolic links
        ln -s /opt/google/${INSTALL_DIR}/chrome /usr/bin/google-chrome && \
    # Test installation
        google-chrome --version && \
    # Clean up
        apt-get autoclean && \
        apt-get clean && \
        apt-get autoremove && \
        rm -Rf /tmp/* && \
        rm -Rf /var/tmp/* && \
        rm -Rf /var/lib/apt/lists/*