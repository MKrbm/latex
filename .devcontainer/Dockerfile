ARG VARIANT="ubuntu-22.04"

FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-${VARIANT} AS base_vscode

ARG USERNAME=user
ARG GROUPNAME=user

RUN usermod -l user vscode
RUN groupmod -n user vscode
RUN echo "root:root" | chpasswd
RUN echo "$USERNAME:$USERNAME" | chpasswd
RUN usermod -d /home/$USERNAME -m $USERNAME
RUN usermod -aG sudo user 

WORKDIR /home/${USERNAME}/project

# # # install some utilities
RUN apt-get update -y && apt-get -y install git software-properties-common htop cmake openssh-server

RUN apt-get update -q \
    && apt-get install -qy build-essential wget libfontconfig1 \
    && rm -rf /var/lib/apt/lists/*

# Install TexLive with scheme-basic
RUN wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz; \
	mkdir /install-tl-unx; \
	tar -xvf install-tl-unx.tar.gz -C /install-tl-unx --strip-components=1; \
    echo "selected_scheme scheme-basic" >> /install-tl-unx/texlive.profile; \
	/install-tl-unx/install-tl -profile /install-tl-unx/texlive.profile; \
    rm -r /install-tl-unx; \
	rm install-tl-unx.tar.gz
RUN git config --global --add safe.directory /home/${USERNAME}/project


FROM base_vscode as base-amd64
ENV PATH="/usr/local/texlive/2022/bin/x86_64-linux:${PATH}"
RUN tlmgr install latexmk
RUN tlmgr install scheme-medium
RUN tlmgr install collection-langjapanese collection-langcjk xcomment


FROM base_vscode as base-arm64
ENV PATH="/usr/local/texlive/2022/bin/aarch64-linux:${PATH}"
RUN tlmgr install latexmk
RUN tlmgr install scheme-medium
RUN tlmgr install collection-langjapanese collection-langcjk xcomment

FROM base-${TARGETARCH}
USER $USERNAME