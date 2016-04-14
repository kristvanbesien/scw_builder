## -*- docker-image-name: "rhel7:latest" -*-
FROM rhel7/rhel
# following 'FROM' lines are used dynamically thanks do the image-builder
# which dynamically update the Dockerfile if needed.


# Environment
ENV SCW_BASE_IMAGE rhel7:latest


# Adding and calling builder-enter
COPY ./overlay-${ARCH}/etc/yum.repos.d/ /etc/yum.repos.d/
COPY ./overlay-image-tools/usr/local/sbin/scw-builder-enter /usr/local/sbin/
RUN set -e; case "${ARCH}" in \
    armv7l|armhf|arm) \
        echo "ARM not supported by RHEL7"\
        exit 1\ 
      ;; \
    x86_64|amd64) \
        yum install -y redhat-lsb-core; \
        /bin/sh -e /usr/local/sbin/scw-builder-enter; \
        yum clean all; \
      ;; \
    esac


RUN if [ "$ARCH" = "armv7l" ]; then YUM_OPTS=--nogpg; fi \
 && yum install ${YUM_OPTS} -y \
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
      mg \
      ntp \
      ntpdate \
      rsync \
      screen \
      socat \
      ssh \
      sudo \
      sysstat \
      tar \
      tcpdump \
      tmux \
      vim \
      wget \
 && yum clean all


# Patch rootfs
COPY ./overlay-image-tools ./overlay ./overlay-${ARCH} /


# Enable Scaleway services
RUN systemctl enable \
	scw-generate-ssh-keys \
	scw-fetch-ssh-keys \
	scw-gen-machine-id \
	scw-kernel-check \
	scw-sync-kernel-modules


# Clean rootfs from image-builder
RUN /usr/local/sbin/scw-builder-leave
