FROM jenkins/jenkins:lts-jdk17

USER root
ARG DOCKER_COMPOSE_VERSION=2.29.7
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl git maven docker-cli \
    && mkdir -p /usr/local/lib/docker/cli-plugins \
    && arch="$(dpkg --print-architecture)" \
    && case "$arch" in \
         amd64) compose_arch="x86_64" ;; \
         arm64) compose_arch="aarch64" ;; \
         *) echo "Arquitectura no soportada para Docker Compose: $arch" >&2; exit 1 ;; \
       esac \
    && curl -fsSL "https://github.com/docker/compose/releases/download/v${DOCKER_COMPOSE_VERSION}/docker-compose-linux-${compose_arch}" \
         -o /usr/local/lib/docker/cli-plugins/docker-compose \
    && chmod +x /usr/local/lib/docker/cli-plugins/docker-compose \
    && groupadd -f docker \
    && usermod -aG docker jenkins \
    && rm -rf /var/lib/apt/lists/*

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

USER jenkins
