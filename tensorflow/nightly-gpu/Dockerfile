FROM eboraas/jupyter
MAINTAINER Ed Boraas <ed@boraas.ca>

#Note: ci.tensorflow.org does not seem to support TLS. Please understand that by running
#this image, you are blindly trusting whomever was claiming to be ci.tensorflow.org at
#build time.
RUN apt-get update && \
    /usr/local/bin/pip --no-cache-dir install --upgrade 'http://ci.tensorflow.org/view/Nightly/job/nigntly-matrix-linux-gpu/TF_BUILD_CONTAINER_TYPE=GPU,TF_BUILD_IS_OPT=OPT,TF_BUILD_IS_PIP=PIP,TF_BUILD_PYTHON_VERSION=PYTHON2,label=gpu-linux/lastSuccessfulBuild/artifact/pip_test/whl/tensorflow-0.9.0-cp27-none-linux_x86_64.whl' || \
    /usr/local/bin/pip --no-cache-dir install --upgrade 'http://ci.tensorflow.org/view/Nightly/job/nigntly-matrix-linux-gpu/TF_BUILD_CONTAINER_TYPE=GPU,TF_BUILD_IS_OPT=OPT,TF_BUILD_IS_PIP=PIP,TF_BUILD_PYTHON_VERSION=PYTHON2,label=gpu-linux/lastSuccessfulBuild/artifact/pip_test/whl/tensorflow-0.8.0-cp27-none-linux_x86_64.whl' && \
    rm -r /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

CMD ["/bin/sh", "-c", "/usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks"]

