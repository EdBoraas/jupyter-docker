FROM eboraas/tensorflow:stable
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install xvfb zlib1g python-opengl ffmpeg build-essential git swig python-dev cmake zlib1g-dev && \
    cd /tmp/ && \
    git clone https://github.com/openai/gym && \
    cd gym/ && \
    /usr/local/bin/pip --no-cache-dir install --upgrade '.[all]' && \
    dpkg --purge build-essential git swig python-dev cmake zlib1g-dev && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/* /root/.cache/pip/ /tmp/gym/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

RUN echo '#!/bin/bash' > /tmp/openai-gym.sh && \
    echo '/usr/bin/xvfb-run -s "-screen 0 1280x720x24" /usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks' >> /tmp/openai-gym.sh && \
    chmod +x /tmp/openai-gym.sh
CMD ["/tmp/openai-gym.sh"]
