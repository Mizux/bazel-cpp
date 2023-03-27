# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/r/opensuse/tumbleweed
FROM opensuse/tumbleweed AS env
# Install system build dependencies
ENV PATH=/usr/local/bin:$PATH
RUN zypper update -y \
&& zypper install -y git gcc gcc-c++ zlib-devel bazel \
&& zypper clean -a
ENV CC=gcc CXX=g++

FROM env AS devel
WORKDIR /home/project
COPY . .

FROM devel AS build
RUN bazel version
RUN bazel build \
 -c opt \
 --action_env=BAZEL_CXXOPTS="-std=c++17" \
 --subcommands=true \
 ...

FROM build AS test
RUN bazel test \
 -c opt \
 --action_env=BAZEL_CXXOPTS="-std=c++17" \
 --test_output=all \
 ...
