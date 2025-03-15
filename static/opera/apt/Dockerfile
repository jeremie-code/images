# TODO: to fix the opera installation

FROM browsers/base:7.4.2

ARG VERSION
ARG PACKAGE=opera-stable
# opera-stable / opera-beta / opera-developer
ARG INSTALL_DIR=opera

LABEL browser=$PACKAGE:$VERSION
LABEL maintainer="jeremie-code"

# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

RUN \
    # Set APT sources
        curl -fsSL https://deb.opera.com/archive.key | gpg --dearmor | tee /usr/share/keyrings/opera.gpg > /dev/null && \
        echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/opera.gpg] https://deb.opera.com/opera-stable/ stable non-free' | tee -a /etc/apt/sources.list.d/opera-stable.list && \
        # Install required packages
        apt-get update && \
        apt-get -y --no-install-recommends install \
        iproute2 \
        $PACKAGE=$VERSION && \
        # Download and install libffmpeg.so
        curl -L -o /tmp/0.97.0-linux-x64.zip https://github.com/nwjs-ffmpeg-prebuilt/nwjs-ffmpeg-prebuilt/releases/download/0.97.0/0.97.0-linux-x64.zip && \
        unzip /tmp/0.97.0-linux-x64.zip -d /tmp/0.97.0-linux-x64 && \
        mkdir -p /usr/lib/x86_64-linux-gnu/ && \
        mv /tmp/0.97.0-linux-x64/libffmpeg.so /usr/lib/x86_64-linux-gnu/ && \
        # Update library cache
        ldconfig && \
        # Fix default argument
        ( \
            # sed -i -e 's@exec -a "$0" "$HERE/opera"@& --no-sandbox --disable-gpu@' /usr/bin/$PACKAGE || \
            sed -i -e 's@exec -a "$0" "$HERE/opera"@& --no-sandbox --disable-gpu@' /usr/bin/opera \
        ) && \
        # Fix permission
        if [ -f /usr/lib/$INSTALL_DIR/opera_sandbox ]; then \
            chown root:root /usr/lib/$INSTALL_DIR/opera_sandbox && \
            chmod 4755 /usr/lib/$INSTALL_DIR/opera_sandbox; \
        fi && \
        # Symbolic links
        ( [ "$PACKAGE" != "opera-stable" ] && ln /usr/bin/$PACKAGE /usr/bin/opera ) || true && \
        # Test installation
        opera --version && \
    # Clean up
        apt-get autoclean && \
        apt-get clean && \
        apt-get autoremove && \
        rm -Rf /tmp/* && \
        rm -Rf /var/tmp/* && \
        rm -Rf /var/lib/apt/lists/*



# =====================================================================================
# ===================================================================================
# =====================================================================================

# FROM browsers/base:7.4.2

# ARG VERSION
# ARG PACKAGE=opera-stable
# ARG INSTALL_DIR=opera

# LABEL browser=$PACKAGE:$VERSION
# LABEL maintainer="jeremie-code"

# # Set non-interactive mode for apt-get
# ENV DEBIAN_FRONTEND=noninteractive


# RUN \
#     # Download Opera
#     if [ "$PACKAGE" = "opera-stable" ]; then \
#         curl -fSLo /tmp/opera.deb https://get.geo.opera.com/pub/opera/desktop/${VERSION}/linux/${PACKAGE}_${VERSION}_amd64.deb; \
#     elif [ "$PACKAGE" = "opera-beta" ]; then \
#         curl -fSLo /tmp/opera.deb https://get.geo.opera.com/pub/opera-beta/${VERSION}/linux/${PACKAGE}_${VERSION}_amd64.deb; \
#     elif [ "$PACKAGE" = "opera-developer" ]; then \
#         curl -fSLo /tmp/opera.deb https://get.geo.opera.com/pub/opera-developer/${VERSION}/linux/${PACKAGE}_${VERSION}_amd64.deb; \
#     fi && \
#     chmod +x /tmp/opera.deb && \
#     # Install required packages
#     apt-get update && \
#     apt-get -y --no-install-recommends install \
#     iproute2 \
#     /tmp/opera.deb && \
#     # Download and install libffmpeg.so
#     curl -L -o /tmp/0.97.0-linux-x64.zip https://github.com/nwjs-ffmpeg-prebuilt/nwjs-ffmpeg-prebuilt/releases/download/0.97.0/0.97.0-linux-x64.zip && \
#     unzip /tmp/0.97.0-linux-x64.zip -d /tmp/0.97.0-linux-x64 && \
#     mkdir -p /usr/lib/x86_64-linux-gnu/ && \
#     mv /tmp/0.97.0-linux-x64/libffmpeg.so /usr/lib/x86_64-linux-gnu/ && \
#     # Update library cache
#     ldconfig && \
#     # Fix default argument
#     ( \
#         # sed -i -e 's@exec -a "$0" "$HERE/opera"@& --no-sandbox --disable-gpu@' /usr/bin/$PACKAGE || \
#         sed -i -e 's@exec -a "$0" "$HERE/opera"@& --no-sandbox --disable-gpu@' /usr/bin/opera \
#     ) && \
#     # Fix permission
#     if [ -f /usr/lib/$INSTALL_DIR/opera_sandbox ]; then \
#         chown root:root /usr/lib/$INSTALL_DIR/opera_sandbox && \
#         chmod 4755 /usr/lib/$INSTALL_DIR/opera_sandbox; \
#     fi && \
#     # Symbolic links
#     ( [ "$PACKAGE" != "opera-stable" ] && ln /usr/bin/$PACKAGE /usr/bin/opera ) || true && \
#     # Test installation
#     opera --version && \
#     # Clean up
#     apt-get autoclean && \
#     apt-get clean && \
#     apt-get autoremove && \
#     rm -Rf /tmp/* && \
#     rm -Rf /var/tmp/* && \
#     rm -Rf /var/lib/apt/lists/*
