# [Choice] Node.js version: 16, 14, 12
ARG VARIANT=16
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-${VARIANT}

ARG USERNAME=node
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ENV npm config set registry https://registry.npmjs.org/

# RUN su node -c "umask 0002 && npm install -g http-server @vue/cli @vue/cli-service-global"
WORKDIR /workspaces/cardano_docker_sandbox2
COPY ./ ./
# Alter node user as needed, install tslint, typescript. eslint is installed by javascript image
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
    groupmod --gid $USER_GID $USERNAME \
    && usermod --uid $USER_UID --gid $USER_GID $USERNAME \
    && chown -R $USER_UID:$USER_GID /home/$USERNAME; \
    fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    && apt-get update -y \
    && apt-get -y install libsecret-1-dev \
    && apt-get -y install acl

RUN chown -R ${USERNAME}:${USERNAME} .devcontainer/scripts/ \
    && chmod +x -R .devcontainer/scripts/
RUN ./.devcontainer/scripts/4-helm
RUN ./.devcontainer/scripts/6-python
RUN ./.devcontainer/scripts/5-docker