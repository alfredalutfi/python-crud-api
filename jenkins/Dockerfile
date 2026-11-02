# Image Jenkins untuk lab macOS (Docker Desktop) / Linux.
# Image resmi jenkins/jenkins tidak memiliki python3 dan Docker CLI,
# dan binary docker milik macOS tidak bisa di-mount ke container Linux.
FROM jenkins/jenkins:lts-jdk17
USER root
RUN apt-get update \
 && apt-get install -y --no-install-recommends python3 curl \
 && rm -rf /var/lib/apt/lists/*
COPY --from=docker:27-cli /usr/local/bin/docker /usr/local/bin/docker
USER jenkins
