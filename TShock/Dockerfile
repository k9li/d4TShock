FROM alpine:3.15.0 AS base

RUN apk add --update-cache \
    unzip

# add the bootstrap file
COPY bootstrap.sh /tshock/bootstrap.sh

# Download and unpack TShock
ADD https://github.com/Pryaxis/TShock/releases/download/v4.5.17/TShock4.5.17_Terraria_1.4.3.6.zip /
RUN unzip *.zip -d /tshock && \    
    rm *.zip && \
    chmod +x /tshock/TerrariaServer.exe && \
    # add executable perm to bootstrap
    chmod +x /tshock/bootstrap.sh

FROM mono:slim

LABEL maintainer="K"

# documenting ports
EXPOSE 7777 7878

# env used in the bootstrap
ENV CONFIGPATH=/data
ENV LOGPATH=/data/logs
ENV WORLD_FILENAME=""

# Allow for external data
VOLUME ["/data"]

# install nuget to grab tshock dependencies
# RUN apt-get update -y && \
#     apt-get install -y nuget mariadb-client jq && \
#     rm -rf /var/lib/apt/lists/* /tmp/*

# copy game files
COPY --from=base /tshock/ /tshock/

# Set working directory to server
WORKDIR /tshock

# run the bootstrap, which will copy the TShockAPI.dll before starting the server
ENTRYPOINT [ "/bin/sh", "bootstrap.sh" ]
