Bootstrap: shub
From: mcw-rcc/rstudio:1.1.456

%labels
Maintainer Matthew Flister
Version 09.20.18

%help
This container runs Tensorflow-GPU 1.10 for R.

%environment
    # nvidia driver libs specific cuda version libs are mounted by --bind command at run
    # required for GPU enabled container
    SHELL=/bin/bash
    CPATH="/cuda/include:$CPATH"
    PATH="/cuda/bin:/nvidia:$PATH"
    LD_LIBRARY_PATH="/cuda/lib64:/nvidia:$LD_LIBRARY_PATH"
    CUDA_HOME="/cuda"
    LC_ALL="C"
    export PATH LD_LIBRARY_PATH CPATH CUDA_HOME LC_ALL

%post
    # default mount points
    mkdir -p /scratch/global /scratch/local /rcc/stor1/refdata /rcc/stor1/projects /rcc/stor1/depts
    
    # NVIDIA driver mount points
    mkdir /nvidia /cuda
    touch /usr/bin/nvidia-smi

    # Install necessary packages
    apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        gcc-multilib \
        libatlas-base-dev \
        libboost-all-dev \
        libhdf5-serial-dev \
        libprotobuf-dev \
        protobuf-compiler \
        libopenblas-dev \
        liblapack-dev \
        gfortran \
        libcurl4-openssl-dev \
        python-pip \
        python3-pip \
        pkg-config \
        python-dev \
        python3-dev \
        python-setuptools \
        python3-setuptools \
    apt-get clean

    # Update pip
    pip install --no-cache-dir --upgrade pip==9.0.3
    pip3 install --no-cache-dir --upgrade pip==9.0.3

    # Install TensorFlow-GPU
    export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.10.0-cp27-none-linux_x86_64.whl
    pip install --no-cache-dir --ignore-installed --upgrade $TF_BINARY_URL

    # Install python packages
    pip install --no-binary --upgrade keras tflearn numpy nibabel h5py scikit-learn pandas scipy matplotlib ipykernel jupyter jupyterlab pydicom opencv-python tables virtualenv

    # Install TensorFlow-GPU
    export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.10.0-cp35-cp35m-linux_x86_64.whl
    pip3 install --no-cache-dir --ignore-installed --upgrade $TF_BINARY_URL

    # Install python packages
    pip3 install --no-binary --upgrade keras tflearn numpy nibabel h5py scikit-learn pandas scipy matplotlib ipykernel jupyter jupyterlab pydicom opencv-python tables virtualenv

    # Install rstudio/tensorflow
    R -e 'install.packages("devtools", repos="https://cran.rstudio.com"); options(unzip = "internal"); devtools::install_github("rstudio/tensorflow"); library(tensorflow); install_tensorflow()'
%test
    R -e 'sess = tf$Session(); hello <- tf$constant('Hello, TensorFlow!'); sess$run(hello)'
