ARG BASE_IMAGE=ubuntu:20.04

FROM $BASE_IMAGE

ARG QEMU_ARCH=x86_64

ARG NEW_USER=$NEW_USER
ARG NEW_PASWD=$NEW_PASWD

ADD x86_64_qemu-$QEMU_ARCH-static.tar.gz /usr/bin

COPY . /bd_build

RUN /bd_build/prepare.sh && \
    /bd_build/system_services.sh && \
    /bd_build/utilities.sh && \
    /bd_build/cleanup.sh

#For resolv syslog-ng problem
RUN sed -i 's/version: 3.13/version: 3.25/g' /etc/syslog-ng/syslog-ng.conf

RUN useradd -rm -d /home/$NEW_USER -s /bin/bash -g root -G sudo -u 1000 $NEW_USER
RUN echo $NEW_USER:$NEW_PASWD | chpasswd

#Port Exposed
EXPOSE 22/tcp

ENV DEBIAN_FRONTEND="teletype" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LC_ALL="en_US.UTF-8"

CMD ["/sbin/my_init"]
