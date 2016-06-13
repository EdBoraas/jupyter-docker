# Note: this Dockerfile inherits from eboraas/tensorflow:build-env in
# order to ensure that the build process stays within Docker Hub's
# 2 hour build window. The downside to inheriting from build-env is
# that the image ends up much larger, due to the retained intermediate
# build images. I've set this Dockerfile up in such a way that it can
# also inherit from eboraas/jupyter without any further changes. The
# build time will be slightly longer, but the image will be much more
# compact.
# TL;DR: You want to use "FROM eboraas/jupyter"

#FROM eboraas/jupyter
FROM eboraas/tensorflow:build-env
MAINTAINER Ed Boraas <ed@boraas.ca>

# Docker Hub currently limits automated builds to 1 CPU and 2GB of RAM.
# As such, I set the default concurrency to 2 jobs to try to avoid resource
# starvation. If you're building this yourself, you'll want to increase
# this value appropriately. This can be done by editing this Dockerfile, or
# at build time using, e.g.,  `docker build --build-arg CONCURRENT_JOBS=8`
ARG CONCURRENT_JOBS=2

# Note: Docker Hub automated builds sometimes fail on Bazel trying to pull from GitHub; nasty
# hack included to try multiple times, in place until Bazel incorporates retry support.
ENV PYTHON_BIN_PATH /usr/bin/python
ENV TF_NEED_GCP 0
ENV TF_NEED_CUDA 0
RUN apt-get update && \
    apt-get -y install openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    cd /usr/src/ && \
    if [ -z $(which bazel) ]; then \
        cd /tmp/ && \
        curl --location --silent --show-error --remote-name https://github.com/bazelbuild/bazel/releases/download/0.3.0/bazel-0.3.0-installer-linux-x86_64.sh && \
        echo '91cae68d1b200a00c982df204a285042c104a3bfe629af5e8d0f289418f60468  bazel-0.3.0-installer-linux-x86_64.sh' |sha256sum --check --quiet && \
        chmod +x bazel-0.3.0-installer-linux-x86_64.sh && \
        ./bazel-0.3.0-installer-linux-x86_64.sh && \
        rm bazel-0.3.0-installer-linux-x86_64.sh && \
        git clone --recurse-submodules --single-branch --depth=1 https://github.com/tensorflow/tensorflow ; \
    fi && \
    cd tensorflow && \
    ./configure && \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package || \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package || \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package || \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package || \
    bazel --batch build --noshow_progress --spawn_strategy=standalone --genrule_strategy=standalone -c opt --copt="-march=sandybridge" --copt="-mtune=sandybridge" --copt="-mavx" --define=use_fast_cpp_protos=true -j ${CONCURRENT_JOBS} tensorflow/tools/pip_package:build_pip_package && \
    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg && \
    /usr/local/bin/pip --no-cache-dir install --upgrade /tmp/tensorflow_pkg/*.whl && \
    dpkg --purge bazel openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/tensorflow/ /tmp/tensorflow_pkg/ /root/.cache/bazel/ /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

