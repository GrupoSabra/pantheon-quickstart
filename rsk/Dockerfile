#default version to latest
ARG PANTHEON_VERSION=latest

FROM sabradevregistry01.azurecr.io/rsk-private:rsk-private.1.1.4

# Copy new entrypoint scripts
COPY *_start.sh /opt/rsk-private/

# Install a dos 2 unix EOL converter (supporting either alpine or ubuntu images)
RUN (apt-get update && apt-get install dos2unix || apk add --update dos2unix) && \
    rm -rf /var/cache/apt/* && rm -rf /var/cache/apk/*

# Run dos2unix EOL conversion on all shell scripts to prevent scripts to fail if edited with a windows IDE
# that rewrote the EOL to CRLF before we build the image. See issue #4
RUN find /opt/rsk-private/*.sh -type f -print0 | xargs -0 dos2unix

# specify default entrypoint to start the node
ENTRYPOINT ["/opt/rsk-private/node_start.sh"]
