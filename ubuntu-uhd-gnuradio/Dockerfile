# ettusresearch/ubuntu-uhd-gnuradio:18.04

# Provides a base Ubuntu (18.04) image with the latest (as of 2 May 2020) UHD and GNU Radio installed

FROM        ubuntu:18.04
MAINTAINER  Ettus Research

# Last build date - this can be updated whenever there are security updates so
# that everything is rebuilt
ENV         security_updates_as_of 2020-05-01

# This will make apt-get install without question
ARG         DEBIAN_FRONTEND=noninteractive
ARG         UHD_TAG=v3.15.0.0 
ARG	    GR_TAG=v3.8.1.0 
ARG         MAKEWIDTH=2

# Install security updates and required packages
RUN         apt-get update
RUN         apt-get -y install -q \
                build-essential \
                apt-utils \
                ccache \
                git \
                python3-dev \
                python3-pip \
                curl
# Install UHD and GNU Radio dependencies for Ubuntu 18.04
# Mostly based on the now-obsolete AN-445 on the Ettus Research Knowledge Base: 
#https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux 
# There are probably some unneeded Python2 and other random packages that get installed in this step.  Meh.
RUN         apt-get -y install -q \
			swig \
			cmake \
			doxygen \
			libboost-all-dev \
			libtool \
			libusb-1.0-0 \
			libusb-1.0-0-dev \
			libudev-dev \
			libncurses5-dev \
			libfftw3-bin \
			libfftw3-dev \
			libfftw3-doc \
			libcppunit-1.14-0 \
			libcppunit-dev \
			libcppunit-doc \
			ncurses-bin \
			cpufrequtils \
			python-numpy \
			python-numpy-doc \
			python-numpy-dbg \
			python-scipy \
			python-docutils \
			python3-numpy \
			python3-docutils \
			python3-mako \
			python3-requests\
			dpdk \
			libdpdk-dev \
			qt4-bin-dbg \
			qt4-default \
			qt4-doc \
			libqt4-dev \
			libqt4-dev-bin \
			python-qt4 \
			python-qt4-dbg \
			python-qt4-dev \
			python-qt4-doc \
			python-qt4-doc \
			libqwt6abi1 \
			libfftw3-bin \
			libfftw3-dev \
			libfftw3-doc \
			ncurses-bin \
			libncurses5 \
			libncurses5-dev \
			libncurses5-dbg \
			libfontconfig1-dev \
			libxrender-dev \
			libpulse-dev \
			g++ \
			automake \
			autoconf \
			python-dev \
			libcppunit-dev \
			fort77 \
			libsdl1.2-dev \
			python-wxgtk3.0 \
			ccache \
			python-opengl \
			libgsl-dev \
			python-cheetah \
			python-mako \
			python-lxml \
			doxygen \
			qt4-dev-tools \
			libqwtplot3d-qt5-dev \
			pyqt4-dev-tools \
			python-qwt5-qt4 \
			wget \
			libxi-dev \
			gtk2-engines-pixbuf \
			r-base-dev \
			python-tk \ 
			liborc-0.4-0 \
			liborc-0.4-dev \
			libasound2-dev \
			python-gtk2 \
			libzmq3-dev \
			libzmq5 python-requests \
			python-sphinx \
			libcomedi-dev \
			python-zmq \
			libqwt-dev \
			libqwt6abi1 \
			python-six \
			libgps-dev \
			libgps23 \
			gpsd \
			gpsd-clients \
			python-gps \
			liblog4cpp5-dev \
			thrift-compiler \
			python-thrift \
			jack \
		        python-click \
		        python-click-plugins \
			libcodec2-dev \
			portaudio19-dev \
			libgsm-tools \
			libgsm1-dev \
			libgmp10 \
			libgmp-dev \
			python-setuptools
# More dependencies brought to you by GNU Radio v3.8.x.x.
RUN	     apt-get -y install -q \
		python3-yaml \
		python3-gi \
		python3-gi-cairo \
		gir1.2-gtk-3.0 \
	        libqt5opengl5-dev \
	        python3-pyqt5 \ 
	        pyqt5-dev-tools \ 
	        qttools5-dev-tools \ 
	        libqwt-qt5-dev \
		python3-click \
		python3-click-plugins 
RUN          rm -rf /var/lib/apt/lists/*
WORKDIR      /usr/local/src/gnuradio/build
RUN          git clone https://github.com/EttusResearch/uhd.git /usr/local/src/uhd
RUN          cd /usr/local/src/uhd/ && git checkout $UHD_TAG
RUN          mkdir -p /usr/local/src/uhd/host/build
WORKDIR      /usr/local/src/uhd/host/build
RUN          cmake .. -DUHD_RELEASE_MODE=release -DCMAKE_INSTALL_PREFIX=/usr
RUN          make -j $MAKEWIDTH
RUN          make install
RUN          ldconfig
RUN          uhd_images_downloader
RUN	     git clone https://github.com/gnuradio/volk.git /usr/local/src/volk
RUN	     cd /usr/local/src/volk 
RUN	     mkdir -p /usr/local/src/volk/build
WORKDIR      /usr/local/src/volk/build
RUN	     cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 -DCMAKE_INSTALL_PREFIX=/usr ../
RUN	     make -j $MAKEWIDTH
RUN	     make install
RUN	     ldconfig
RUN	     git clone https://github.com/gnuradio/gnuradio.git /usr/local/src/gnuradio
RUN	     cd /usr/local/src/gnuradio && git checkout $GR_TAG
RUN	     mkdir -p /usr/local/src/gnuradio/build
RUN	     cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 -DCMAKE_INSTALL_PREFIX=/usr -DENABLE_INTERNAL_VOLK=OFF ../
RUN	     make -j $MAKEWIDTH
RUN	     make install
RUN	     ldconfig
WORKDIR      /

