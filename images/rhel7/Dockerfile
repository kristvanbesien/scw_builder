## -*- docker-image-name: "rhel7:latest" -*-
FROM rhel7

# Environment
ENV SCW_BASE_IMAGE rhel7


# Adding and calling builder-enter
COPY ./overlay/usr/local/sbin/ /usr/local/sbin/
RUN  /bin/sh -e /usr/local/sbin/scw-builder-enter 

# Install stuff...
RUN yum install ${YUM_OPTS} -y \
      bash \
      bash-completion \
      ca-certificates \
      cron \
      curl \
      ethstatus \
      haveged \
      ioping \
      iotop \
      iperf \
      locate \
      make \
      ntp \
      ntpdate \
      rsync \
      screen \
      socat \
      sudo \
      sysstat \
      tar \
      tcpdump \
      tmux \
      vim \
      wget \
      openssh-clients \
      openssh-server \
 && yum clean all

# Patch rootfs
COPY ./overlay/ /

# Enable Scaleway services
RUN systemctl enable \
	scw-generate-ssh-keys \
	scw-fetch-ssh-keys \
	scw-gen-machine-id \
	scw-kernel-check \
	scw-sync-kernel-modules 

# Enable sshd so we can log in.
RUN systemctl enable sshd


# Clean rootfs from image-builder
RUN /usr/local/sbin/scw-builder-leave
