FROM openjdk:8-jdk as builder

RUN apt-get -qq update && \
    apt-get -qq -y install git && \
    apt-get -qq -y install libsodium-dev

RUN mkdir /tmp/orion
COPY ./orion*.tar.gz /tmp/orion

WORKDIR /tmp/orion

RUN tar xzf orion*.tar.gz \
  && rm orion*.tar.gz \
  && mv orion* build

FROM openjdk:8-jre

RUN apt-get -qq update && \
    apt-get -qq -y install libsodium-dev

COPY --from=builder /tmp/orion/* /orion/

WORKDIR orion

COPY data /orion/data

ENV DATA_FOLDER = data/data1
ENTRYPOINT ./orion/bin/orion $DATA_FOLDER/orion.conf
