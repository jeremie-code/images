# https://support.mozilla.org/en-US/kb/install-firefox-linux#w_install-firefox-deb-package-for-debian-based-distributions
FROM browsers/base:7.4.2

# Set environment variables to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Define build arguments
ARG VERSION=134
ARG PACKAGE=firefox

# Add a label for the browser package and version
LABEL browser=$PACKAGE:$VERSION

# Install necessary packages
RUN apt-get update && \
    apt-get install -y wget gnupg2 && \
    rm -rf /var/lib/apt/lists/*

# Create a directory for APT repository keys
RUN install -d -m 0755 /etc/apt/keyrings

# Import the Mozilla APT repository signing key
RUN wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
# RUN gpg -n -q --import --import-options import-show /etc/apt/keyrings/packages.mozilla.org.asc | awk '/pub/{getline; gsub(/^ +| +$/,""); if($0 == "35BAA0B33E9EB396F59CA838C0BA5CE6DC6315A3") print "\nThe

# Add the Mozilla APT repository to the sources list
RUN echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null

# Configure APT to prioritize packages from the Mozilla repository
RUN echo 'Package: *\nPin: origin packages.mozilla.org\nPin-Priority: 1000' | tee /etc/apt/preferences.d/mozilla

# Update package list and install Firefox with the specified version
RUN apt-get update && \
    apt-get install -y $PACKAGE && \
    ( [ "$PACKAGE" != "firefox" ] && ln /usr/bin/$PACKAGE /usr/bin/firefox ) || true && \
    firefox --version && \
    rm -rf /var/lib/apt/lists/*

# =====================================================================================
# =====================================================================================
# =====================================================================================

FROM browsers/base:7.4.2
ARG VERSION
ARG PACKAGE=firefox
ARG INSTALL_DIR=firefox
LABEL browser=$PACKAGE:$VERSION
LABEL maintainer="jeremieb"
# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive
RUN \ 
    # Install required packages
        apt-get update && \
        apt-get -y --no-install-recommends install \
        iproute2 \
        libavcodec58 \
        libgtk-3-0 \
        libstartup-notification0 \
        libdbus-glib-1-2 \
        dbus \
        curl && \
    # Download selenium manager
        curl -fSLo /tmp/selenium-manager-linux https://github.com/SeleniumHQ/selenium_manager_artifacts/releases/download/selenium-manager-372cb5d/selenium-manager-linux && \
        chmod +x /tmp/selenium-manager-linux && \
        output=$(/tmp/selenium-manager-linux --browser firefox --browser-version $VERSION --avoid-stats --output JSON) && \
        browser_path=$(echo "$output" | jq -r '.result.browser_path') && \
        browser_path=${browser_path%/*} && \
        mkdir -p /usr/lib/$INSTALL_DIR/ && \
        mv ${browser_path}/* /usr/lib/$INSTALL_DIR/ && \
        rm /tmp/selenium-manager-linux && \
    # Symbolic links
        ln -s /usr/lib/$INSTALL_DIR/firefox /usr/bin/firefox && \
    # Test installation
        firefox --version && \
    # Clean up
        apt-get autoclean && \
        apt-get clean && \
        apt-get autoremove && \
        rm -Rf /tmp/* && \
        rm -Rf /var/tmp/* && \
        rm -Rf /var/lib/apt/lists/*
