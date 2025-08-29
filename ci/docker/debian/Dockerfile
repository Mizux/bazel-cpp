# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/_/debian
FROM debian:latest AS env

# Install system build dependencies
RUN apt-get update -qq \
&& apt-get install -yq git wget curl libssl-dev build-essential \
&& apt-get install -yq default-jdk \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# Install Bazelisk
ARG TARGETARCH=amd64
RUN wget \
https://github.com/bazelbuild/bazelisk/releases/download/v1.27.0/bazelisk-linux-${TARGETARCH} \
&& chmod +x bazelisk-linux-${TARGETARCH} \
&& mv bazelisk-linux-${TARGETARCH} /usr/local/bin/bazel

FROM env AS devel
WORKDIR /home/project
COPY . .

FROM devel AS build
RUN bazel version
RUN bazel build \
 -c opt \
 --subcommands=pretty_print \
 ...

FROM build AS test
RUN bazel test \
 -c opt \
 --test_output=errors \
 ...
