# CI build tools for Kurento projects - Dockerfile
#
# This Docker image is used to run all CI jobs related to Kurento projects. It
# contains all tools needed for the different repositories, such as
# [git-buildpackage][1] to generate Debian packages, Maven for Java projects,
# and Node.js for JavaScript projects.
#
# [1]: https://hub.docker.com/r/kurento/kurento-buildpackage
#
#
# Build command
# -------------
#
# docker build [Args...] --tag kurento/kurento-ci-buildtools:<UbuntuVersion> .
#
#
# Build arguments
# ---------------
#
# --build-arg UBUNTU_VERSION=<UbuntuVersion>
#
#   <UbuntuVersion> is like "xenial", "bionic", etc.
#
#   Optional. Default: "xenial".

ARG UBUNTU_VERSION=xenial

FROM kurento/kurento-buildpackage:${UBUNTU_VERSION}

# Settings from parent image:
# * Re-enable automatically cleaning the apt cache
ENV APT_KEEP_CACHE="false"

# Install Node.js 8.x LTS (includes NPM) and Bower
# Used by JavaScript-based projects
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
 && apt-get update && apt-get install --yes \
        nodejs \
 && rm -rf /var/lib/apt/lists/* \
 && npm -g install bower \
 && npm cache clean --force

# Install packages
# * default-jdk (Java JDK): For Java projects
# * maven: For Java projects
# * jq, jshon: For some JavaScript tools and jobs
# * rsync: For remote copies
#
# Tools used by kurento-java/kurento-integration-tests/kurento-test:
# * ffmpeg: For different integration testst that generate an RTP stream
# * libmediainfo-dev: MediaInfoLibrary class
# * netcat: KmsService class, to check if KMS port is already in use
RUN apt-get update && apt-get install --yes \
        default-jdk \
        maven \
        jq jshon \
        rsync \
        ffmpeg \
        libmediainfo-dev \
        netcat-openbsd \
 && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/bin/bash"]
CMD []
