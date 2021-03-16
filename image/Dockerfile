ARG BASE_IMAGE=ubuntu:20.04

FROM $BASE_IMAGE

ARG NEW_USER=$NEW_USER
ARG NEW_PASWD=$NEW_PASWD
ARG QEMU_ARCH=$QEMU_ARCH

ADD x86_64_qemu-$QEMU_ARCH-static.tar.gz /usr/bin

COPY . /bd_build

RUN /bd_build/prepare.sh && \
    /bd_build/system_services.sh && \
    /bd_build/utilities.sh && \
    /bd_build/cleanup.sh

#Cryrcit -> Personalized Dockerfile

#For resolv syslog-ng problem
RUN sed -i 's/version: 3.13/version: 3.25/g' /etc/syslog-ng/syslog-ng.conf
RUN sed -i 's/use_dns(no);/use_dns(no); dns-cache(no);/g' /etc/syslog-ng/syslog-ng.conf

#For enable SSH on startup
RUN rm -f /etc/service/sshd/down

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh
RUN service ssh restart


#Add user
RUN useradd -rm -d /home/$NEW_USER -s /bin/bash -g root -G sudo -u 1000 $NEW_USER
RUN echo $NEW_USER:$NEW_PASWD | chpasswd

#Port Exposed
EXPOSE 22/tcp

#<-- Cryrcit

ENV DEBIAN_FRONTEND="teletype" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LC_ALL="en_US.UTF-8"

CMD ["/sbin/my_init"]
