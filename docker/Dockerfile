FROM ubuntu:xenial
ENV VERSION 4.2.3-1

RUN apt-get update && apt-get install -y \
    wget \
    lsb-release \
    procps \
    apt-transport-https
RUN wget -O- https://mirror.oxfordnanoportal.com/apt/ont-repo.pub | apt-key add -
RUN PLATFORM=$(lsb_release -cs) && echo "deb http://mirror.oxfordnanoportal.com/apt ${PLATFORM}-stable non-free" | tee /etc/apt/sources.list.d/nanoporetech.sources.list
RUN apt-get update && apt-get install -y ont-guppy-cpu=$VERSION~xenial

ENV NANOCOMPORE_PIPELINE=true
ENV PYTHONNOUSERSITE="not_empty_to_avoid_loading_packages_from_user_home"
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda-10.2/compat/"

RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-latest-Linux-x86_64.sh -b \
    && rm -f Miniconda3-latest-Linux-x86_64.sh
ENV PATH="/root/miniconda3/bin:${PATH}"
RUN conda --version

RUN conda create -n nanocompore -y -c bioconda -c conda-forge pysam pandas pip nanocompore htslib samtools nanopolish nanopolishcomp pycoqc bedparse bedtools minimap2
ENV PATH="/root/miniconda3/envs/nanocompore/bin:${PATH}"

RUN /bin/bash -c "conda init bash"

RUN apt-get clean && apt-get purge -y wget lsb-release && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp


