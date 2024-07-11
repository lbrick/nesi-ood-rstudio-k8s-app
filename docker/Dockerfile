FROM rocker/geospatial:latest

# run unminimise, e.g. for man pages to work
RUN yes | unminimize

# install kubectl, required for running on the k8s cluster
ARG KUBECTL_VERSION=v1.28.5
RUN curl -LO https://dl.k8s.io/release/$KUBECTL_VERSION/bin/linux/amd64/kubectl \
    && mv kubectl /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl

# copy in ood k8s utils
ARG UTILS_HASH=6298fb01f7a7c66a8454e3f0fd74437a32491423
RUN git clone https://github.com/nesi/training-environment-k8s-utils.git /opt/ood-k8s-utils \
    && cd /opt/ood-k8s-utils \
    && git checkout $UTILS_HASH \
    && chmod +x /opt/ood-k8s-utils/files/* \
    && mv /opt/ood-k8s-utils/files/* /bin/ \
    && rm -rf /opt/ood-k8s-utils

# make a dummy module command to avoid warnings from ondemand job_script_content.sh
RUN echo "#!/bin/bash" > /bin/module \
    && chmod +x /bin/module

# install packages, some of these are required to make the container behave like ldap client
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        curl \
        dnsutils \
        git \
        jq \
        ldap-utils \
        libnss-ldapd \
        libpam-ldapd \
        less \
        nano \
        nodejs \
        nscd \
        nslcd \
        python-is-python3 \
        python3 \
        python3-pip \
        rsync \
        unzip \
        vim \
        wget \
        zip \
    && rm -rf /var/lib/apt/lists/*

# install lmod
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        lua5.3 \
        lua-bit32:amd64 \
        lua-posix:amd64 \
        lua-posix-dev \
        liblua5.3-0:amd64 \
        liblua5.3-dev:amd64 \
        tcl \
        tcl-dev \
        tcl8.6 \
        tcl8.6-dev:amd64 \
        libtcl8.6:amd64
RUN wget https://sourceforge.net/projects/lmod/files/Lmod-8.7.tar.bz2 \
    && tar -xjvf Lmod-8.7.tar.bz2
WORKDIR /Lmod-8.7
RUN ./configure --prefix=/opt/apps \
    && make install \
    && ln -s /opt/apps/lmod/lmod/init/profile /etc/profile.d/z00_lmod.sh \
    && ln -s /opt/apps/lmod/lmod/init/cshrc /etc/profile.d/z00_lmod.csh

# clean up
RUN rm -f /Lmod-8.7.tar.bz2
WORKDIR /

# install R packages
RUN Rscript -e 'install.packages("vegan")'
