FROM eboraas/debian:stable
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install pkg-config python3 python3-pip && \
    pip3 install --no-cache-dir --ignore-installed --upgrade pip && \
    for module in $(/usr/local/bin/pip3 list --no-cache-dir --outdated | tail -n +3 | cut -f1 -d' '); do /usr/local/bin/pip3 install --no-cache-dir --ignore-installed --upgrade ${module}; done || \
    /usr/local/bin/pip3 install --no-cache-dir --ignore-installed --upgrade jupyter && \
    dpkg --purge pkg-config python3-pip && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

WORKDIR /mnt/notebooks
EXPOSE 8888

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

