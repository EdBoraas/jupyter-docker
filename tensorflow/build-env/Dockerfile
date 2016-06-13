FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

# This Dockerfile builds an image suitable for doing custom builds of TensorFlow, either
# interactively or by inheriting from this image. Note that resulting images will have
# a larger footprint, since the build environment is present in the image.
# For an example of a custom build inheriting from this image, see ../custom-example/Dockerfile

# Install prerequisite official packages
RUN apt-get update && \
    apt-get -y install openjdk-8-jdk-headless pkg-config zip g++ zlib1g-dev unzip bash-completion curl git swig python-dev python-wheel && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*

# Download and install official Bazel .deb package
WORKDIR /tmp
RUN curl --location --silent --show-error --remote-name https://github.com/bazelbuild/bazel/releases/download/0.3.0/bazel-0.3.0-installer-linux-x86_64.sh && \
    echo '91cae68d1b200a00c982df204a285042c104a3bfe629af5e8d0f289418f60468  bazel-0.3.0-installer-linux-x86_64.sh' |sha256sum --check --quiet && \
    chmod +x bazel-0.3.0-installer-linux-x86_64.sh && \
    ./bazel-0.3.0-installer-linux-x86_64.sh && \
    rm bazel-0.3.0-installer-linux-x86_64.sh

# Clone the official TensorFlow repo and have Bazel pull depencies
WORKDIR /usr/src
RUN git clone --recurse-submodules https://github.com/tensorflow/tensorflow
WORKDIR /usr/src/tensorflow
RUN bazel --batch fetch --noshow_progress //tensorflow/tools/pip_package:build_pip_package

CMD ["/bin/bash"]

