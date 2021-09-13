FROM fedora:34

# set versions
ARG MINICONDA_VERSION
ARG MD5
ARG CS_VERSION
ARG DOCKER_USER

# set environment
ENV TZ=Europe/Amsterdam
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PATH=/opt/conda/bin:$PATH

# install system packages
RUN dnf -y update && \
    dnf -y install \
    wget \
    git \
    bzip2 \
    passwd \
    zsh \
    systemd

# install Python with miniconda3
RUN curl -fL https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh -o /tmp/miniconda.sh  && \
    echo -e "${MD5} */tmp/miniconda.sh" | md5sum -c - && \
    bash /tmp/miniconda.sh -f -b -p /opt/conda && \
    # init conda
    /opt/conda/bin/conda init && \
    # install python packages to base environment
    /opt/conda/bin/conda install --yes -c conda-forge \
    pandas

# install code-server
RUN curl -fL https://github.com/cdr/code-server/releases/download/v${CS_VERSION}/code-server-${CS_VERSION}-amd64.rpm -o /tmp/code-server.rpm && \
    rpm -i /tmp/code-server.rpm

### config taken from official dockerfile
# configure user
RUN adduser -m coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd 
USER 1000
ENV USER=${DOCKER_USER}

# init conda for DOCKER_USER
RUN /opt/conda/bin/conda init

# expose standard port
EXPOSE 8080

WORKDIR /home
COPY entrypoint.sh /usr/bin/entrypoint.sh
ENTRYPOINT ["/usr/bin/entrypoint.sh"]
