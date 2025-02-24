FROM browsers/base:7.4.2

ARG VERSION
ARG PACKAGE=microsoft-edge-stable
# microsoft-edge-stable / microsoft-edge-beta / microsoft-edge-dev
ARG INSTALL_DIR=msedge

LABEL browser=$PACKAGE:$VERSION
LABEL maintainer="jeremie-code"

# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

RUN \
    # Set APT sources
        curl -fSsL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | tee -a /usr/share/keyrings/microsoft-edge.gpg > /dev/null && \
        echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main' | tee /etc/apt/sources.list.d/microsoft-edge.list && \
    # Install required packages
        apt-get update && \
        apt-get -y --no-install-recommends install \
        iproute2 \
        $PACKAGE=$VERSION && \
    # Fix default argument
        ( \
            sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/$PACKAGE || \
            sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/microsoft-edge \
        ) && \
    # Fix permission
        if [ -f /opt/microsoft/$INSTALL_DIR/msedge-sandbox ]; then \
            chown root:root /opt/microsoft/$INSTALL_DIR/msedge-sandbox && \
            chmod 4755 /opt/microsoft/$INSTALL_DIR/msedge-sandbox; \
        fi && \
    # Test installation
    microsoft-edge --version && \
    # Clean up
        apt-get autoclean && \
        apt-get clean && \
        apt-get autoremove && \
        rm -Rf /tmp/* && \
        rm -Rf /var/tmp/* && \
        rm -Rf /var/lib/apt/lists/*
# =====================================================================================
# =====================================================================================
# =====================================================================================

# FROM browsers/base:7.4.2

# ARG VERSION
# ARG PACKAGE=microsoft-edge-stable
# # microsoft-edge-stable / microsoft-edge-beta / microsoft-edge-dev
# ARG INSTALL_DIR=msedge

# LABEL browser=$PACKAGE:$VERSION
# LABEL maintainer="jeremie-code"

# # Set non-interactive mode for apt-get
# ENV DEBIAN_FRONTEND=noninteractive

# RUN \
#     # Download edge
#     curl -fSLo /tmp/edge.deb https://packages.microsoft.com/repos/edge/pool/main/m/${PACKAGE}/${PACKAGE}_${VERSION}-1_amd64.deb && \
#     chmod +x /tmp/edge.deb && \
#     # Install required packages
#         apt-get update && \
#         apt-get -y --no-install-recommends install \
#         iproute2 \
#         /tmp/edge.deb && \
#     # Fix default argument
#         ( \
#             sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/$PACKAGE || \
#             sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/microsoft-edge \
#         ) && \
#     # Fix permission
#         if [ -f /opt/microsoft/$INSTALL_DIR/msedge-sandbox ]; then \
#             chown root:root /opt/microsoft/$INSTALL_DIR/msedge-sandbox && \
#             chmod 4755 /opt/microsoft/$INSTALL_DIR/msedge-sandbox; \
#         fi && \
#     # Test installation
#     microsoft-edge --version && \
#     # Clean up
#         apt-get autoclean && \
#         apt-get clean && \
#         apt-get autoremove && \
#         rm -Rf /tmp/* && \
#         rm -Rf /var/tmp/* && \
#         rm -Rf /var/lib/apt/lists/*

# # =====================================================================================
# # =====================================================================================
# # =====================================================================================

# FROM browsers/base:7.4.2

# ARG VERSION
# ARG PACKAGE=microsoft-edge-stable
# ARG INSTALL_DIR=msedge

# LABEL browser=$PACKAGE:$VERSION
# LABEL maintainer="jeremie-code"

# # Set non-interactive mode for apt-get
# ENV DEBIAN_FRONTEND=noninteractive

# RUN \ 
#     # Install required packages
#         apt-get update && \
#         apt-get -y --no-install-recommends install \
#         libatk-bridge2.0-0 \
#         libatomic1 \
#         libatspi2.0-0 \
#         libcairo-gobject2 \
#         libcolord2 \
#         libepoxy0 \
#         libgbm1 \
#         libgtk-3-0 \
#         libgtk-3-common \
#         liblcms2-2 \
#         librest-0.7-0 \
#         libsoup-gnome2.4-1 \
#         libwayland-client0 \
#         libwayland-cursor0 \
#         libwayland-egl1 \
#         libwayland-server0 \
#         libxkbcommon0 \
#         xdg-utils \
#         ca-certificates \
#         fonts-liberation \
#         libappindicator3-1 \
#         libnss3 \
#         lsb-base \
#         libcurl4 \
#         iproute2 \
#         curl && \
#     # Download selenium manager
#         curl -fSLo /tmp/selenium-manager-linux https://github.com/SeleniumHQ/selenium_manager_artifacts/releases/download/selenium-manager-372cb5d/selenium-manager-linux && \
#         chmod +x /tmp/selenium-manager-linux && \
#         output=$(/tmp/selenium-manager-linux --browser edge --browser-version $VERSION --avoid-stats --output JSON) && \
#         browser_path=$(echo "$output" | jq -r '.result.browser_path') && \
#         browser_path=${browser_path%/*} && \
#         mkdir -p /opt/microsoft/$INSTALL_DIR/ && \
#         mv ${browser_path}/* /opt/microsoft/$INSTALL_DIR/ && \
#     # Fix default argument
#         ( \
#             sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/$PACKAGE || \
#             sed -i -e 's@exec -a "$0" "$HERE/msedge"@& --no-sandbox --disable-gpu@' /opt/microsoft/$INSTALL_DIR/microsoft-edge \
#         ) && \
#     # Fix permission
#         if [ -f /opt/microsoft/$INSTALL_DIR/msedge-sandbox ]; then \
#             chown root:root /opt/microsoft/$INSTALL_DIR/msedge-sandbox && \
#             chmod 4755 /opt/microsoft/$INSTALL_DIR/msedge-sandbox; \
#         fi && \
#     # Symbolic links
#         ln -s /opt/microsoft/$INSTALL_DIR/microsoft-edge /usr/bin/microsoft-edge && \
#     # Test installation
#         microsoft-edge --version && \
#     # Clean up
#       apt-get autoclean && \
#       apt-get clean && \
#       apt-get autoremove && \
#       rm -Rf /tmp/* && \
#       rm -Rf /var/tmp/* && \
#       rm -Rf /var/lib/apt/lists/*
 