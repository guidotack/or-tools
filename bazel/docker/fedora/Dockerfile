# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/_/fedora
FROM fedora:latest AS env
LABEL maintainer="corentinl@google.com"
# Install system build dependencies
ENV PATH=/usr/local/bin:$PATH
RUN dnf -y update \
&& dnf -y install git \
&& dnf -y groupinstall "Development Tools" \
&& dnf -y install gcc-c++ zlib-devel \
&& dnf -y install dnf-plugins-core \
&& dnf -y copr enable vbatts/bazel \
&& dnf -y install bazel \
&& dnf clean all
CMD [ "/usr/bin/bash" ]

FROM env AS devel
WORKDIR /home/lib
COPY . .

FROM devel as build
RUN bazel build --curses=no --copt='-Wno-sign-compare' //...:all

FROM build as test
RUN bazel test -c opt --curses=no --copt='-Wno-sign-compare' //...:all
