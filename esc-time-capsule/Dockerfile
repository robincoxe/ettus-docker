# ettusresearch/esc-time-capsule:18.04

# Provides a base Ubuntu (18.04) image with latest UHD installed

FROM        ubuntu:18.04
MAINTAINER  Robin Coxe <coxe@quanttux.com>

# Last build date - this can be updated whenever there are security updates so
# that everything is rebuilt
ENV         security_updates_as_of 2020-08-09

# This will make apt-get install without question
ARG         DEBIAN_FRONTEND=noninteractive
ARG         UHD_TAG=v3.14.0.0
ARG         MAKEWIDTH=2

# Install security updates and required packages
RUN         apt-get update
RUN         apt-get -y install -q \
                build-essential \
                ccache \
                git \
                python3-dev \
                python3-pip \
                curl
# Install UHD dependencies
RUN         apt-get -y install -q \
                libboost-all-dev \
                libusb-1.0-0-dev \
                libudev-dev \
                python3-mako \
                doxygen \
                python3-docutils \
                cmake \
                python3-requests \
                python3-numpy \
                dpdk \
                libdpdk-dev
#Install Vivado XSIM and MIG dependencies 
RUN          apt-get install -y \
             wget \
             libglib2.0-0 \
             libsm6 \
             libxi6 \
             libxrender1 \
             libxrandr2 \
             libfreetype6 \
             libfontconfig 

RUN          rm -rf /var/lib/apt/lists/*

RUN          mkdir -p /usr/local/src
RUN          git clone https://github.com/EttusResearch/uhd.git /usr/local/src/uhd
RUN          cd /usr/local/src/uhd/ && git checkout $UHD_TAG
RUN          mkdir -p /usr/local/src/uhd/host/build
WORKDIR      /usr/local/src/uhd/host/build
RUN          cmake .. -DENABLE_PYTHON3=ON -DUHD_RELEASE_MODE=release -DCMAKE_INSTALL_PREFIX=/usr
RUN          make -j $MAKEWIDTH
RUN          make install
WORKDIR      /
# copy in config file
COPY install_config.txt /

# download and run the Vivado install
ARG VIVADO_TAR_HOST=host_ip:8000
ARG VIVADO_TAR_FILE=Xilinx_Vivado_SDK_2017.4_1216_1
ARG VIVADO_VERSION=2017.4
RUN echo "Downloading ${VIVADO_TAR_FILE} from ${VIVADO_TAR_HOST}" && \
  wget ${VIVADO_TAR_HOST}/${VIVADO_TAR_FILE}.tar.gz -q 
#Can copy Vivado tarball directly as an alternative to wget.
#COPY ${VIVADO_TAR_FILE}.tar.gz /
RUN  echo "Extracting Vivado tar file" && \
  tar xzf ${VIVADO_TAR_FILE}.tar.gz && \
  /${VIVADO_TAR_FILE}/xsetup --agree 3rdPartyEULA,WebTalkTerms,XilinxEULA --batch Install --config install_config.txt && \
  rm -rf ${VIVADO_TAR_FILE}*

#add vivado tools to path (root)
RUN echo "source /opt/Xilinx/Vivado/${VIVADO_VERSION}/settings64.sh" >> /root/.profile

#copy in the license file (root)
RUN mkdir -p /root/.Xilinx
COPY Xilinx.lic /root/.Xilinx/

#make a Vivado user
RUN adduser --disabled-password --gecos '' vivado
USER vivado
WORKDIR /home/vivado
#add vivado tools to path
RUN echo "source /opt/Xilinx/Vivado/${VIVADO_VERSION}/settings64.sh" >> /home/vivado/.bashrc

#copy in the license file for vivado user
RUN mkdir /home/vivado/.Xilinx
COPY Xilinx.lic /home/vivado/.Xilinx/
