FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

# Docker Hub currently limits automated builds to 1 CPU and 2GB of RAM.
# As such, I set the default concurrency to 2 jobs to try to avoid resource
# starvation. If you're building this yourself, you'll want to increase
# this value appropriately. This can be done by editing this Dockerfile, or
# at build time using, e.g.,  `docker build --build-arg CONCURRENT_JOBS=8`
ARG CONCURRENT_JOBS=2

ENV PYTHON_BIN_PATH /usr/bin/python
ENV TF_NEED_GCP 0
ENV TF_NEED_CUDA 0
RUN apt-get update && \
    apt-get -y install openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion wget git swig python-dev python-wheel && \
    cd /tmp/ && \
    wget --no-verbose https://github.com/bazelbuild/bazel/releases/download/0.2.2/bazel_0.2.2-linux-x86_64.deb && \
    dpkg -i --force-depends bazel_0.2.2-linux-x86_64.deb && \
    git clone --recurse-submodules --single-branch --depth=1 https://github.com/tensorflow/tensorflow && \
    cd /tmp/tensorflow && \
    ./configure && \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package && \
    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg && \
    /usr/local/bin/pip --no-cache-dir install --upgrade /tmp/tensorflow_pkg/*.whl && \
    dpkg --purge bazel openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion wget git swig python-dev python-wheel && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/* /tmp/bazel_0.2.2-linux-x86_64.deb /tmp/tensorflow/ /tmp/tensorflow_pkg/ /root/.cache/bazel/ /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

