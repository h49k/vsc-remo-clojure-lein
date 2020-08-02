FROM clojure:openjdk-15-lein-2.9.3

ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV JOKER_VERSION 0.15.5
ENV SHELL /bin/bash

ARG USERNAME=clojure
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils 2>&1 \
    #
    # Verify git, process tools, lsb-release (common in install instructions for CLIs) installed
    && apt-get -y install curl unzip git sudo procps lsb-release \
    #
    # && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    #
    # install joker
    # https://www.kbaba1001.com/entry/2019/07/13/210715
    && curl -OL https://github.com/candid82/joker/releases/download/v${JOKER_VERSION}/joker-${JOKER_VERSION}-linux-amd64.zip \
    && unzip joker-${JOKER_VERSION}-linux-amd64.zip \
    && rm joker-${JOKER_VERSION}-linux-amd64.zip \
    && chmod a+x joker && mv joker /usr/bin \
    # Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME
    # [Optional] Add sudo support for the non-root user

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog
