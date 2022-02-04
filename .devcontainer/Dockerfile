ARG PYTHON_VERSION="3.10.1"

FROM python:${PYTHON_VERSION}-slim

LABEL maintainer="n98gt56ti@gmail.com"

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends git openssh-client

ARG UID=1000
ARG GID=1000
ENV UID=${UID}
ENV GID=${GID}
ARG USERNAME=vscode
ARG GROUPNAME=${USERNAME}
RUN groupadd --gid $UID $GROUPNAME \
    && useradd --uid $GID --gid $USERNAME --shell /bin/bash --create-home $GROUPNAME
USER $UID

# for persisting VScode extensions
RUN mkdir -p /home/$USERNAME/.vscode-server/extensions \
        /home/$USERNAME/.vscode-server-insiders/extensions \
    && chown -R $USERNAME \
        /home/$USERNAME/.vscode-server \
        /home/$USERNAME/.vscode-server-insiders
ARG WORKSPACE_FOLDER
# for persisting cli history
RUN SNIPPET="export PROMPT_COMMAND='history -a' && \
export HISTFILE=/home/$USERNAME/commandhistory/.bash_history && \
export HISTSIZE= && \
export HISTFILESIZE= && \
shopt -s histappend && \
alias ll='ls -ahlF --color' && \
alias llt=\"ls --time-style='+%d-%m-%Y %H:%M:%S' -l\" && \
bind '\"\e[A\": history-search-backward' && \
bind '\"\e[B\": history-search-forward' && \
stty -ixon && \
[ -f ${WORKSPACE_FOLDER:?Shoul be set and non-empty}/.devcontainer/.bashrc_kustom ] && . ${WORKSPACE_FOLDER:?Shoul be set and non-empty}/.devcontainer/.bashrc_kustom " \
    && mkdir /home/$USERNAME/commandhistory \
    && touch /home/$USERNAME/commandhistory/.bash_history \
    && echo $SNIPPET >> "/home/$USERNAME/.bashrc"
