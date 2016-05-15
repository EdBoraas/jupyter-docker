FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

#Note: 0.8.0 has an issue where the bundled sklearn conflicts with the system sklearn in some cases.
#Until a newer version of TensorFlow is released as stable, I'm removing sklearn from eboraas/tensorflow:stable
#to prevent segfaults
RUN apt-get update && \
    /usr/local/bin/pip --no-cache-dir uninstall -y scikit-learn && \
    /usr/local/bin/pip --no-cache-dir install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl && \
    rm -r /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

