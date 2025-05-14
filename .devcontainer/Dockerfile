FROM ubuntu:noble

RUN if id "ubuntu" &>/dev/null; then \
        echo "Deleting user 'ubuntu'  for noble" && userdel -f -r ubuntu || echo "Failed to delete ubuntu user for noble"; \
    else \
        echo "User 'ubuntu' does not exist for noble"; \
    fi;

RUN apt-get update && apt-get install -y

# Install Dependencies
RUN apt install zsh curl unzip git wget man-db -y \
    && yes | unminimize 2>&1 \
    && rm -rf /var/lib/apt/lists/*

ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000
ARG FNM_PATH=/home/${USERNAME}/.fnm

RUN groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd --uid ${USER_UID} --gid ${USER_GID} --create-home --shell /bin/bash ${USERNAME}
RUN usermod --shell "$(which zsh)" ${USERNAME}

USER ${USERNAME}
WORKDIR /home/${USERNAME}

# Download and install fnm:
RUN curl -fsSL https://fnm.vercel.app/install | bash -s -- --install-dir ${FNM_PATH} --skip-shell

# Setup Node.js:
ENV PATH="${FNM_PATH}:${PATH}"
 
RUN fnm install 22 \
    && eval $(fnm env) \
    && fnm use 22 \
    && corepack enable pnpm \
    && corepack prepare pnpm@latest --activate

RUN fnm env >> /home/${USERNAME}/.bashrc

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended || true