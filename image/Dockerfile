ARG BASE_IMAGE=ubuntu:20.04
FROM $BASE_IMAGE

ARG QEMU_ARCH
ADD x86_64_qemu-${QEMU_ARCH}-static.tar.gz /usr/bin

COPY . /bd_build

RUN /bd_build/prepare.sh && \
	/bd_build/system_services.sh && \
	/bd_build/utilities.sh && \
	/bd_build/cleanup.sh

ENV DEBIAN_FRONTEND="teletype" \
    LANG="it_IT.UTF-8" \
    LANGUAGE="it_IT:en" \
    LC_ALL="it_IT.UTF-8"

CMD ["/sbin/my_init"]