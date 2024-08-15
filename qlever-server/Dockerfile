FROM ubuntu:latest
MAINTAINER Patrick Westphal

RUN apt update
RUN apt -y upgrade
RUN apt -y install git build-essential cmake ninja-build libicu-dev libboost-iostreams-dev libboost-program-options-dev libboost-url-dev libssl-dev libzstd-dev python3-pip

USER ubuntu
WORKDIR /home/ubuntu/qlever
RUN git clone https://github.com/ad-freiburg/qlever.git .
RUN cmake -DCMAKE_BUILD_TYPE=Release -DLOGLEVEL=INFO -DUSE_PARALLEL=true -D_NO_TIMING_TESTS=ON -GNinja . && ninja -j1 # -j1 is just to avoid compiling too many things in parallel and running out of memory

VOLUME /data

ENV DATASET_NAME example
ENV RDF_FILE example.ttl
ENV MAX_MEM 10g
ENV NUM_SIMULTANEOUS_QUERIES 5
ENV SERVER_PORT 8080
ENV ACCESS_TOKEN abcdefg

ENTRYPOINT ./IndexBuilderMain --index-basename "$DATASET_NAME" --file-format ttl --kg-input-file /data/"$RDF_FILE" && ./ServerMain --index-basename "$DATASET_NAME" --port $SERVER_PORT --num-simultaneous-queries $NUM_SIMULTANEOUS_QUERIES --memory-max-size "$MAX_MEM" --access-token "$ACCESS_TOKEN"

## example run command:
## docker run -p 8080:8080 --env DATASET_NAME=go --env RDF_FILE=go.ttl --volume /tmp/data/go/:/data:ro
