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
RUN curl --location --silent --show-error --remote-name https://github.com/bazelbuild/bazel/releases/download/0.2.2/bazel-0.2.2-installer-linux-x86_64.sh && \
    echo '3213c9d2339892fec911897ef93e0e5bbbf90b2d482daa9c4a5ef51ad0b6e165  bazel-0.2.2-installer-linux-x86_64.sh' |sha256sum --check --quiet && \
    chmod +x bazel-0.2.2-installer-linux-x86_64.sh && \
    ./bazel-0.2.2-installer-linux-x86_64.sh && \
    rm bazel-0.2.2-installer-linux-x86_64.sh

# By default, clone the official TensorFlow repo and run a shell suitable for building/working interactively
WORKDIR /usr/src
CMD ["/bin/bash", "-c", "git clone --recurse-submodules https://github.com/tensorflow/tensorflow && cd tensorflow && /bin/bash"]

