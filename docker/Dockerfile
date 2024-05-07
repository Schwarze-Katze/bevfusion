FROM nvidia/cuda:11.3.1-devel-ubuntu20.04

RUN apt-get update && apt-get install wget -yq
RUN apt-get install build-essential g++ gcc -y
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get install libgl1-mesa-glx libglib2.0-0 -y
RUN apt-get install openmpi-bin openmpi-common libopenmpi-dev libgtk2.0-dev git -y

# Install miniconda
ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
     /bin/bash ~/miniconda.sh -b -p /opt/conda
# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH
RUN conda install python=3.8 -y
RUN conda install pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -y
RUN pip install setuptools==58.0.4
RUN pip install Pillow==8.4.0
RUN pip install tqdm
RUN pip install torchpack
RUN pip install mmcv==1.4.0 mmcv-full==1.4.0 mmdet==2.20.0
RUN pip install nuscenes-devkit
RUN pip install mpi4py==3.0.3
RUN pip install numba==0.48.0

RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts

ENV ID_FILE id_rsa
COPY --chown=root:root ~/.ssh/${ID_FILE} /root/.ssh/${ID_FILE}
RUN chmod 600 /root/.ssh/${ID_FILE}

# configure github config
ENV USERNAME name
ENV USEREMAIL email
RUN git clone git@github.com:Schwarze-Katze/bevfusion.git /bevfusion && \
    git config --global user.name ${USERNAME} && \
    git config --global user.email ${USEREMAIL}

WORKDIR /bevfusion
RUN bash ./tools/download_pretrained.sh