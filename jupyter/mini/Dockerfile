FROM eboraas/debian:stretch
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install pkg-config python python-pip libffi6 libfreetype6 libpng16-16 libjpeg62-turbo libffi-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev python-tk && \
    pip install --no-cache-dir --upgrade --ignore-installed pip && \
    for module in $(/usr/local/bin/pip list --no-cache-dir --outdated |cut -f1 -d' '); do /usr/local/bin/pip install --no-cache-dir --upgrade ${module}; done && \
    /usr/local/bin/pip install --no-cache-dir --upgrade numpy ipykernel matplotlib jupyter && \
    dpkg --purge pkg-config python-pip libffi-dev libfreetype6-dev libpng-dev libjpeg62-turbo-dev && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

# Pre-populate font list to avoid warning on first import of matplotlib.pyplot 
RUN python -c "import matplotlib.pyplot"

WORKDIR /mnt/notebooks
EXPOSE 8888

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

