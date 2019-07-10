FROM eboraas/debian:stable
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install pkg-config python3 python3-pip libffi6 libfreetype6 libpng16-16 libjpeg62-turbo libopenblas-base libffi-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev libopenblas-dev python3-tk && \
    pip3 install --no-cache-dir --ignore-installed --upgrade pip && \
    for module in $(/usr/local/bin/pip3 list --no-cache-dir --outdated | tail -n +3 | cut -f1 -d' '); do /usr/local/bin/pip3 install --no-cache-dir --ignore-installed --upgrade ${module}; done && \
    /usr/local/bin/pip3 install --no-cache-dir --ignore-installed --upgrade numpy scipy sklearn ipykernel matplotlib Pillow jupyter flask && \
    dpkg --purge pkg-config python3-pip libffi-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev libopenblas-dev && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

# Pre-populate font list to avoid warning on first import of matplotlib.pyplot 
RUN python3 -c "import matplotlib.pyplot"

WORKDIR /mnt/notebooks
EXPOSE 8888

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

