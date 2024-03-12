# from latest debain
FROM debian:latest

COPY install-packages upgrade-packages /usr/bin/

RUN  chmod +x /usr/bin/install-packages
RUN  chmod +x /usr/bin/upgrade-packages

### base ###
ARG DEBIAN_FRONTEND=noninteractive

RUN yes | install-packages \
        zip \
        unzip \
        bash-completion \
        build-essential \
        ninja-build \
        htop \
        jq \
        less \
        locales \
        man-db \
        nano \
        ripgrep \
        software-properties-common \
        sudo \
        time \
        emacs-nox \
        vim \
        multitail \
        lsof \
        ssl-cert \
        fish \
        zsh \
        curl \
        python3-launchpadlib \
        git \
    && locale-gen en_US.UTF-8

ENV LANG=en_US.UTF-8

### Update and upgrade the base image ###
RUN upgrade-packages


# ARG USERNAME=rahulvramesh
# ARG USER_UID=1000
# ARG USER_GID=$USER_UID


# RUN groupadd --gid $USER_GID $USERNAME \
#     && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
#     && apt-get update \
#     && apt-get install -y sudo \
#     && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
#     && chmod 0440 /etc/sudoers.d/$USERNAME

# USER $USERNAME

### Gitpod user ###
# '-l': see https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user
RUN useradd -l -u 1002 -G sudo -md /home/rahulvramesh -s /bin/bash -p rahulvramesh rahulvramesh \
    # passwordless sudo for users in the 'sudo' group
    && sed -i.bkp -e 's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' /etc/sudoers
ENV HOME=/home/rahulvramesh
WORKDIR $HOME

# custom Bash prompt
RUN { echo && echo "PS1='\[\033[01;32m\]\u\[\033[00m\] \[\033[01;34m\]\w\[\033[00m\]\$(__git_ps1 \" (%s)\") $ '" ; } >> .bashrc

### Gitpod user (2) ###
USER rahulvramesh
# use sudo so that user does not get sudo usage info on (the first) login
RUN sudo echo "Running 'sudo' for rahulvramesh: success" && \
    # create .bashrc.d folder and source it in the bashrc
    mkdir -p /home/rahulvramesh/.bashrc.d && \
    (echo; echo "for i in \$(ls -A \$HOME/.bashrc.d/); do source \$HOME/.bashrc.d/\$i; done"; echo) >> /home/rahulvramesh/.bashrc


RUN curl -fsSL https://get.jetpack.io/devbox -o devbox_install \
    && chmod +x devbox_install \
    && bash devbox_install -f

# Create New directory called cache
RUN mkdir -p /home/rahulvramesh/.cache

# Run the command to install the devbox
