# FROM browsers/base:7.4.2
# # 
# ARG VERSION
# ARG PACKAGE=firefox
# #  firefox / firefox-esr
# ARG INSTALL_DIR=firefox
# # 
# LABEL browser=$PACKAGE:$VERSION
# LABEL maintainer="jeremie-code"
# # 
# # Set non-interactive mode for apt-get
# ENV DEBIAN_FRONTEND=noninteractive
# # 
# RUN \
#     # Create a directory for APT repository keys
#         install -d -m 0755 /etc/apt/keyrings && \
#     # # Set APT sources
#         curl -fSsL https://packages.mozilla.org/apt/repo-signing-key.gpg -o /etc/apt/keyrings/packages.mozilla.org.asc && \
#         echo 'deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main' | tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null && \
#     # Configure APT to prioritize packages from the Mozilla repository
#         echo 'Package: *\nPin: origin packages.mozilla.org\nPin-Priority: 1000' | tee /etc/apt/preferences.d/mozilla && \
#     # Install required packages
#         apt-get update && \
#         apt-get -y --no-install-recommends install \
#         gnupg2 \
#         iproute2 \
# TODO: find the correct package name for firefox, it doesn't support versioning like chrome or edge
#         $PACKAGE=$VERSION && \
#     # Symbolic links
#         # ( [ "$PACKAGE" != "firefox" ] ln /usr/bin/$PACKAGE /usr/bin/firefox ) || true && \
#     # Test installation
#         firefox --version && \
#     # Clean up
#         apt-get autoclean && \
#         apt-get clean && \
#         apt-get autoremove && \
#         rm -Rf /tmp/* && \
#         rm -Rf /var/tmp/* && \
#         rm -Rf /var/lib/apt/lists/*

# =====================================================================================
# =====================================================================================
# =====================================================================================

FROM browsers/base:7.4.2

ARG VERSION
ARG PACKAGE=firefox
ARG INSTALL_DIR=firefox

LABEL browser=$PACKAGE:$VERSION
LABEL maintainer="jeremie-code"

# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

RUN \
    # Download Firefox
        curl -fSLo /tmp/firefox.deb https://ftp.mozilla.org/pub/firefox/releases/$VERSION/linux-x86_64/en-US/firefox-$VERSION.deb && \
        chmod +x /tmp/firefox.deb && \
    # Install required packages
        apt-get update && \
        apt-get -y --no-install-recommends install \
        gnupg2 \
        iproute2 \
        /tmp/firefox.deb && \
    # Symbolic links
        # ( [ "$PACKAGE" != "firefox" ] ln /usr/bin/$PACKAGE /usr/bin/firefox ) || true && \
    # Test installation
        firefox --version && \
    # Clean up
        apt-get autoclean && \
        apt-get clean && \
        apt-get autoremove && \
        rm -Rf /tmp/* && \
        rm -Rf /var/tmp/* && \
        rm -Rf /var/lib/apt/lists/*

# # =====================================================================================
# # =====================================================================================
# # =====================================================================================

# FROM browsers/base:7.4.2

# ARG VERSION
# ARG PACKAGE=firefox
# ARG INSTALL_DIR=firefox

# LABEL browser=$PACKAGE:$VERSION
# LABEL maintainer="jeremie-code"

# # Set non-interactive mode for apt-get
# ENV DEBIAN_FRONTEND=noninteractive

# RUN \ 
#     # Install required packages
#         apt-get update && \
#         apt-get -y --no-install-recommends install \
#         iproute2 \
#         libavcodec58 \
#         libgtk-3-0 \
#         libstartup-notification0 \
#         libdbus-glib-1-2 \
#         dbus \
#         curl && \
#     # Download selenium manager
#         curl -fSLo /tmp/selenium-manager-linux https://github.com/SeleniumHQ/selenium_manager_artifacts/releases/download/selenium-manager-372cb5d/selenium-manager-linux && \
#         chmod +x /tmp/selenium-manager-linux && \
#         output=$(/tmp/selenium-manager-linux --browser firefox --browser-version $VERSION --avoid-stats --output JSON) && \
#         browser_path=$(echo "$output" | jq -r '.result.browser_path') && \
#         browser_path=${browser_path%/*} && \
#         mkdir -p /usr/lib/$INSTALL_DIR/ && \
#         # mv ${browser_path}/* /usr/lib/$INSTALL_DIR/ && \
#         mv ${browser_path}/* /opt/$INSTALL_DIR/ && \
#     # Symbolic links
#         # ln -s /usr/lib/$INSTALL_DIR/firefox /usr/bin/firefox && \
#         ln -s /opt/$INSTALL_DIR/firefox /usr/local/bin/firefox && \
#     # Test installation
#         firefox --version && \
#     # Clean up
#         apt-get autoclean && \
#         apt-get clean && \
#         apt-get autoremove && \
#         rm -Rf /tmp/* && \
#         rm -Rf /var/tmp/* && \
#         rm -Rf /var/lib/apt/lists/*
