FROM eboraas/openai-gym:prereq-nightly-sandybridge
MAINTAINER Ed Boraas <ed@boraas.ca>

RUN apt-get update && \
    apt-get -y install build-essential swig python-dev cmake zlib1g-dev libsdl2-dev libboost-all-dev wget unzip && \
    /usr/local/bin/pip --no-cache-dir install --upgrade 'gym[all]' && \
    dpkg --purge libsdl2-dev libboost-all-dev wget unzip && \
    apt-get -y autoremove && \
    dpkg --purge build-essential swig python-dev cmake zlib1g-dev && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/* /root/.cache/pip/

# Jupyter Notebook
EXPOSE 8888
# TensorBoard
EXPOSE 6006

WORKDIR /mnt/notebooks

RUN echo '#!/bin/bash' > /tmp/openai-gym.sh && \
    echo '/usr/bin/xvfb-run -s "-screen 0 1280x720x24" /usr/local/bin/jupyter-notebook --no-browser --ip=0.0.0.0 --notebook-dir=/mnt/notebooks' >> /tmp/openai-gym.sh && \
    chmod +x /tmp/openai-gym.sh
CMD ["/tmp/openai-gym.sh"]
