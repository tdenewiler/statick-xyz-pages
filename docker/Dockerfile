FROM ubuntu:20.04

LABEL "name"="Statick"
LABEL "version"="0.0"
LABEL "repository"="https://github.com/sscpac/statick.git"
LABEL "homepage"="https://github.com/sscpac/statick"
LABEL "maintainer"="Thomas Denewiler <tdenewiler@gmail.com>"

ENV DEBIAN_FRONTEND=noninteractive

# Version pinning may be added in the future, but ignore for now.
# hadolint ignore=DL3008
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cccc \
    chktex \
    clang \
    clang-6.0 \
    clang-10 \
    clang-format \
    clang-format-6.0 \
    clang-format-10 \
    clang-tidy \
    clang-tidy-6.0 \
    clang-tidy-10 \
    cmake \
    cppcheck \
    curl \
    file \
    findbugs \
    flawfinder \
    git \
    lacheck \
    libomp-dev \
    libpcre3-dev \
    libperl-critic-perl \
    libxml2 \
    libxml2-utils \
    maven \
    python3-pip \
    python3-venv \
    shellcheck \
    uncrustify \
    wget \
    && rm -rf /var/lib/apt/lists/*
# Version pinning may be added in the future, but ignore for now.
# hadolint ignore=DL3013
RUN python3 -m pip install --no-cache-dir --upgrade pip

RUN python3 -m venv /opt/venv
COPY requirements.txt .
RUN . /opt/venv/bin/activate && pip install -r requirements.txt

# Statick npm tools.
# Newer version is installed from non-apt source due to SSL library compatibility issues.

# Version pinning may be added in the future, but ignore for now.
# hadolint ignore=DL3008,DL3016
RUN curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh && \
    bash nodesource_setup.sh && \
    apt-get update && apt-get install -y --no-install-recommends nodejs && rm -rf /var/lib/apt/lists/* && \
    npm config set prefix -g /usr && \
    npm install -g dockerfilelint && \
    npm install -g dockerfile_lint && \
    npm install -g prettier && \
    npm install -g eslint && \
    npm install -g --prefix $HOME/.node_modules eslint-plugin-html && \
    npm install -g --prefix $HOME/.node_modules eslint-plugin-prettier && \
    npm install -g --prefix $HOME/.node_modules eslint-config-prettier && \
    npm install -g htmllint-cli && \
    npm install -g markdownlint-cli && \
    npm install -g npm-groovy-lint && \
    npm install -g stylelint && \
    npm install -g stylelint-config-standard && \
    npm install -g json-merger

# Install Hadolint binary
RUN mkdir hadolint-bin && \
    curl -sL -o hadolint "https://github.com/hadolint/hadolint/releases/download/v2.6.0/hadolint-$(uname -s)-$(uname -m)" && \
    chmod +x hadolint && \
    mv hadolint hadolint-bin/.
ENV PATH $PWD/hadolint-bin:$PATH

CMD ["bash"]
