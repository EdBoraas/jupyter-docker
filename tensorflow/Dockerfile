FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

# Docker Hub currently limits automated builds to 1 CPU and 2GB of RAM.
# As such, I set the concurrency to 1 job to try to avoid resource
# starvation. If you're building this yourself, you'll want to increase
# this value appropriately.
ENV CONCURRENT_JOBS 1

ENV PYTHON_BIN_PATH /usr/bin/python
ENV TF_NEED_CUDA 0
RUN apt-get update && \
    apt-get -y install openjdk-8-jdk pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    cd /tmp/ && \
    curl --output /tmp/bazel.deb --location https://github.com/bazelbuild/bazel/releases/download/0.2.2/bazel_0.2.2-linux-x86_64.deb && \
    dpkg -i /tmp/bazel.deb && \
    git clone --recurse-submodules https://github.com/tensorflow/tensorflow && \
    cd /tmp/tensorflow && ./configure && \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package && \
    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg && \
    /usr/local/bin/pip --no-cache-dir install --upgrade /tmp/tensorflow_pkg/*.whl && \
    dpkg --purge bazel openjdk-8-jdk pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/* /tmp/bazel.deb /tmp/tensorflow/ /tmp/tensorflow_pkg/ /root/.cache/bazel/ /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

