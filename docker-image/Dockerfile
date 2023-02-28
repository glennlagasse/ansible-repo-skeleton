#
# Usage:
# docker run --rm -it -v $(pwd):/ansible -v ~/.ssh/ansible_id_ed25519:/home/ansible/.ssh/ansible_id_ed25519 glagasse/ansible (ansible | ansible-playbook)
#
ARG PYTHON_IMAGE_TAG="3.11-slim-bullseye"

FROM python:$PYTHON_IMAGE_TAG as base

RUN set -eux; \
    DEBIAN_FRONTEND=noninteractive apt-get update \
    && apt-get --yes dist-upgrade \
    && apt-get --yes --no-install-recommends install git openssh-client vim \
    && rm --recursive --force /var/lib/apt/lists/* \
    && apt-get clean

FROM base as dependencies

RUN set -eux; \
    pip install --upgrade pip setuptools wheel

FROM base

LABEL org.opencontainers.image.authors="glagasse@glagasse.com"

ARG USERNAME=ansible
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN set -eux; \
    groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID --create-home $USERNAME

COPY --from=dependencies /usr/local /usr/local

USER $USERNAME

ENV PATH=/home/$USERNAME/.local/bin:$PATH

COPY --chown=$USERNAME requirements.txt /home/$USERNAME/requirements.txt

RUN set -eux; \
    git config --global --add safe.directory /ansible \
    && pip install --no-cache-dir -r /home/$USERNAME/requirements.txt

WORKDIR /ansible

CMD ["bash"]
