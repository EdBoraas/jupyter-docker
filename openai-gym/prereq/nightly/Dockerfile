FROM eboraas/tensorflow:nightly
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install xvfb zlib1g python-opengl ffmpeg libsdl2-2.0-0 libboost-python1.58.0 libboost-thread1.58.0 libboost-filesystem1.58.0 libboost-system1.58.0 fluidsynth && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

WORKDIR /mnt/notebooks

CMD ["/bin/bash"]
