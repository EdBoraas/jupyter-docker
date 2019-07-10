FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install pkg-config python3-pip && \
    /usr/local/bin/pip3 --no-cache-dir install --upgrade tensorflow && \
    dpkg --purge python3-pip pkg-config && \
    apt-get -y autoremove && \
    rm -r /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

