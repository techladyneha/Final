# Start from Ubuntu base image
FROM ubuntu:22.04

LABEL maintainer="Neha"
LABEL description="Mercor Python SWE Interview - Full Dockerfile from ubuntu:22.04"
LABEL version="1.0"

# Set noninteractive frontend for apt-get to avoid prompts
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Install system dependencies
RUN apt update && apt install -y \
    wget \
    git \
    curl \
    build-essential \
    libffi-dev \
    libtiff-dev \
    python3 \
    python3-pip \
    python-is-python3 \
    jq \
    locales \
    locales-all \
    tzdata \
    nano \
    neovim \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Miniconda
RUN wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh" -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda && \
    rm miniconda.sh

# Add conda to PATH
ENV PATH="/opt/miniconda/bin:$PATH"

# Initialize conda
RUN conda init bash && conda config --set channel_priority strict && conda config --append channels conda-forge

# Create a non-root user
RUN adduser --disabled-password --gecos 'interviewer' nonroot

# Clone Black repo at the specific 2019 commit
ARG GIT_REPO_URL=https://github.com/psf/black
ARG GIT_COMMIT_HASH=d8fa8df0526de9c0968e0a3568008f58eae45364
ARG REPO_DIR=/Mercor_Interview

RUN git clone ${GIT_REPO_URL} ${REPO_DIR} && \
    cd ${REPO_DIR} && \
    git checkout ${GIT_COMMIT_HASH}

WORKDIR ${REPO_DIR}
RUN git config --global --add safe.directory ${REPO_DIR}

# Create the Conda environment and install dependencies
RUN conda create -y -n testbed python=3.7 && \
    conda run -n testbed pip install \
        click==7.0 \
        appdirs==1.4.3 \
        attrs==19.3.0 \
        mypy_extensions==0.4.3 \
        pathspec==0.6.0 \
        regex==2019.11.1 \
        toml==0.10.0 \
        typed-ast==1.4.0 \
        pytest

# Activate testbed environment and make it available
ENV PATH="/opt/miniconda/envs/testbed/bin:$PATH"
