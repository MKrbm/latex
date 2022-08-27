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

ENV PATH="/usr/local/texlive/2022/bin/x86_64-linux:${PATH}"
RUN tlmgr install latexmk
RUN tlmgr install collection-langjapanese collection-langcjk xcomment
RUN git config --global --add safe.directory /home/${USERNAME}/project
USER $USERNAME


# FROM ubuntu:22.04

# ENV DEBIAN_FRONTEND=noninteractive

# # install general packages
# RUN apt-get update && \
#     apt-get install -y --no-install-recommends \
#       build-essential \
#       cpanminus \
#       git \
#       curl \
#       wget \
#       locales && \
#     # clean to reduce image size
#     apt-get clean -y && \
#     apt-get autoremove -y && \
#     apt-get autoclean -y && \
#     rm -rf /var/lib/apt/lists/*

# # install texlive
# RUN mkdir /tmp/install-tl-unx && \
#     curl -L ftp://tug.org/historic/systems/texlive/2020/install-tl-unx.tar.gz | \
#       tar -xz -C /tmp/install-tl-unx --strip-components=1 && \
#     printf "%s\n" \
#       "selected_scheme scheme-basic" \
#       "tlpdbopt_install_docfiles 0" \
#       "tlpdbopt_install_srcfiles 0" \
#       > /tmp/install-tl-unx/texlive.profile && \
#     /tmp/install-tl-unx/install-tl \
#       --profile=/tmp/install-tl-unx/texlive.profile && \
#     rm -rf /tmp/install-tl-unx

# # add TeX Live to PATH
# ENV PATH /usr/local/texlive/2020/bin/x86_64-linux:$PATH

# # install LaTeX packages with tlmgr
# RUN tlmgr install \
#     latexmk \
#     latexindent \
#     collection-latexextra \
#     collection-fontsrecommended \
#     collection-langjapanese

# COPY .latexmkrc /root

# # install perl packages which are requirements of "latexindent"
# RUN cpanm Log::Log4perl Log::Dispatch::File YAML::Tiny File::HomeDir Unicode::GCString

# WORKDIR /workdir
