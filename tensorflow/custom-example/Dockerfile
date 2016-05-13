FROM eboraas/tensorflow:build-env
MAINTAINER Ed Boraas <ed@boraas.ca>

# Docker Hub currently limits automated builds to 1 CPU and 2GB of RAM.
# As such, I set the default concurrency to 2 jobs to try to avoid resource
# starvation. If you're building this yourself, you'll want to increase
# this value appropriately. This can be done by editing this Dockerfile, or
# at build time using, e.g.,  `docker build --build-arg CONCURRENT_JOBS=8`
ARG CONCURRENT_JOBS=2

# Clone the official TensorFlow repo, configure, build, install, then remove all build artifacts
WORKDIR /usr/src
ENV PYTHON_BIN_PATH /usr/bin/python
ENV TF_NEED_GCP 0
ENV TF_NEED_CUDA 0
RUN git clone --recurse-submodules https://github.com/tensorflow/tensorflow && \
    cd tensorflow/ && \
    ./configure && \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package && \
    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg && \
    /usr/local/bin/pip --no-cache-dir install --upgrade /tmp/tensorflow_pkg/*.whl && \
    cd /usr/src && \
    rm -r /usr/src/tensorflow/ /tmp/tensorflow_pkg/ /root/.cache/bazel/ /root/.cache/pip/
# Removing the build environment is optional; it cleans up the resulting image itself, but does not save any net space due to
# the retention of intermediate images.
RUN dpkg --purge openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

