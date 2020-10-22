FROM ubuntu:18.04

# Block prompts
ENV DEBIAN_FRONTEND=noninteractive

# First some commands that are necessary
RUN apt-get update && apt-get install -y gnupg2 software-properties-common libcurl4-gnutls-dev libxml2-dev libssl-dev

# First we will install some dependencies for python and R
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'
RUN add-apt-repository -y ppa:deadsnakes/ppa 
RUN apt update

# Here we go
RUN apt install -y r-base python3.8

# Changing default version of python
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 2

RUN update-alternatives --config python3 --skip-auto

# Installing pip
RUN apt install -y python3-pip python3-setuptools python3-dev

# Installing some basic python packages for me
ADD requirements.txt requirements.txt
RUN pip3 install --upgrade pip \
    && pip install --upgrade -r /requirements.txt \
    && rm /requirements.txt \
    && rm -rf ~/.cache/pip \
    && rm -rf /src


# Installing some important R packages
RUN Rscript -e 'chooseCRANmirror(graphics=FALSE, ind=1); install.packages("ggplot2"); install.packages("dplyr"); install.packages("vsn"); install.packages("hexbin"); install.packages("BiocManager"); BiocManager::install(c("edgeR", "DESeq2", "GenomicAlignments", "rtracklayer", "BiocParallel", "clusterProfiler"));'

# This part is important for using R in jupyter 
RUN Rscript -e 'chooseCRANmirror(graphics=FALSE, ind=1); install.packages("IRkernel"); IRkernel::installspec(user = FALSE)'

