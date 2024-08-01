# see https://stupiddog.jp/note/archives/1125
ARG VARIANT="2023"
FROM public.ecr.aws/amazonlinux/amazonlinux:${VARIANT}

RUN dnf update -y \
    && dnf groupinstall -y "Development tools" \
    && dnf install -y shadow-utils \
        tar \
        gzip \
        sudo \
        bash-completion \
        git \
        which \
        python

RUN dnf install -y zip unzip \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && dnf clean all

RUN dnf install -y gcc \
        zlib-devel \
        bzip2-devel \
        readline-devel \
        sqlite \
        sqlite-devel \
        openssl-devel \
        tk-devel \
        libffi-devel \
        xz-devel

RUN adduser ec2-user
RUN echo 'ec2-user ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER ec2-user
# Python
RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv \
    && echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile \
    && echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile \
    && echo 'eval "$(pyenv init -)"' >> ~/.bash_profile \
    && source ~/.bash_profile

ARG PYTHON_VERSION="3.11.7"
RUN export PYENV_ROOT="$HOME/.pyenv" \
    && command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH" \
    && eval "$(pyenv init -)" \
    && pyenv install ${PYTHON_VERSION} \
    && pyenv global ${PYTHON_VERSION} \
    && pyenv rehash

RUN curl -sSL https://install.python-poetry.org | python - \
    && echo 'export PATH="$HOME/.local/bin:${PATH}"' >> ~/.bash_profile \
    && source ~/.bash_profile