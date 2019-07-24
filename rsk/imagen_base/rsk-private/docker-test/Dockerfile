FROM openjdk:11.0.2-jre-slim-stretch AS builder

RUN mkdir /tmp/rsk
COPY ./rsk-private /tmp/rsk/.
WORKDIR /tmp/rsk
RUN ./gradlew build -x test

RUN mkdir /opt/pantheon
WORKDIR /tmp/rsk/build/distributions/
RUN tar xzf pantheon-*.tar.gz -C /tmp \
  && mv /tmp/pantheon-*/* /opt/pantheon

FROM openjdk:11.0.2-jre-slim-stretch

COPY --from=builder /opt/pantheon /opt/pantheon

WORKDIR /opt/pantheon

# Build-time metadata as defined at http://label-schema.org
ARG BUILD_DATE
ARG VCS_REF
ARG VERSION
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name="RSK" \
      org.label-schema.description="Enterprise Ethereum client" \
      org.label-schema.url="https://docs.pantheon.pegasys.tech/" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/PegaSysEng/pantheon.git" \
      org.label-schema.vendor="Pegasys" \
      org.label-schema.version=$VERSION \
      org.label-schema.schema-version="1.0"

# Expose services ports
# 8545 HTTP JSON-RPC
# 8546 WS JSON-RPC
# 8547 HTTP GraphQL
# 30303 P2P
EXPOSE 8545 8546 8547 30303

ENTRYPOINT ["/opt/pantheon/bin/pantheon"]