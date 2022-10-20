FROM ubuntu:22.04 as base_ubuntu


WORKDIR /home/project
RUN apt-get update -y && apt-get -y install git vim software-properties-common htop cmake openssh-server
COPY terminal-colors-branch.sh /tmp/
RUN cat /tmp/terminal-colors-branch.sh >> ~/.bashrc


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
RUN git config --global --add safe.directory /home/project


FROM base_ubuntu as base-amd64
ENV PATH="/usr/local/texlive/2022/bin/x86_64-linux:${PATH}"
RUN tlmgr install latexmk
RUN tlmgr install scheme-full
RUN tlmgr install collection-langjapanese collection-langcjk xcomment


FROM base_ubuntu as base-arm64
ENV PATH="/usr/local/texlive/2022/bin/aarch64-linux:${PATH}"
RUN tlmgr install latexmk
RUN tlmgr install scheme-full
RUN tlmgr install collection-langjapanese collection-langcjk xcomment

FROM base-${TARGETARCH}
# USER $USERNAME