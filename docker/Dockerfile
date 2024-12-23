FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04


ENV DEBIAN_FRONTEND noninteractive

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub

RUN apt-get update && apt-get install -y \
	python3-opencv ca-certificates python3-dev git wget sudo ninja-build && \
	apt-get clean
RUN ln -sv /usr/bin/python3 /usr/bin/python

# create a non-root user
ARG USER_ID=1000
RUN useradd -m --no-log-init --system  --uid ${USER_ID} appuser -g sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER appuser
WORKDIR /home/appuser

ENV PATH="/home/appuser/.local/bin:${PATH}"
RUN wget https://bootstrap.pypa.io/get-pip.py && \
	python3 get-pip.py --user && \
	rm get-pip.py

# install dependencies
# See https://pytorch.org/ for other options if you use a different version of CUDA
RUN pip install --user tensorboard cmake   # cmake from apt-get is too old
# RUN pip install --user torch==1.8 torchvision==0.9 -f https://download.pytorch.org/whl/cu101/torch_stable.html
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

RUN pip install --user 'git+https://github.com/facebookresearch/fvcore'
# install detectron2
RUN git clone https://github.com/aaa22070338/SwinTextSpotter.git SwinTextSpotter 
# set FORCE_CUDA because during `docker build` cuda is not accessible
ENV FORCE_CUDA="1"
# This will by default build detectron2 for all common cuda architectures and take a lot more time,
# because inside `docker build`, there is no way to tell which architecture will be used.
ARG TORCH_CUDA_ARCH_LIST="Kepler;Kepler+Tesla;Maxwell;Maxwell+Tegra;Pascal;Volta;Turing"
ENV TORCH_CUDA_ARCH_LIST="${TORCH_CUDA_ARCH_LIST}"

RUN pip install --user -e  SwinTextSpotter
RUN pip install numpy==1.26 opencv-python scipy shapely rapidfuzz timm Polygon3

# Set a fixed model cache directory.
ENV FVCORE_CACHE="/tmp"
WORKDIR /home/appuser/SwinTextSpotter

# run detectron2 under user "appuser":
# wget http://images.cocodataset.org/val2017/000000439715.jpg -O input.jpg
# python3 demo/demo.py  \
	#--config-file configs/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x.yaml \
	#--input input.jpg --output outputs/ \
	#--opts MODEL.WEIGHTS detectron2://COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/model_final_f10217.pkl
