FROM nvidia/cudagl:9.0-devel-ubuntu16.04

RUN apt-get update
# System normal libraries
RUN apt-get install -y cmake g++ git ipython minizip python-dev python-h5py python-numpy python-scipy python-sympy qt4-dev-tools \
    && apt-get install -y libassimp-dev libavcodec-dev libavformat-dev libavformat-dev libboost-all-dev libboost-date-time-dev libbullet-dev libfaac-dev libglew-dev libgsm1-dev liblapack-dev liblog4cxx-dev libmpfr-dev libode-dev libogg-dev libpcrecpp0v5 libpcre3-dev libqhull-dev libqt4-dev libsoqt-dev-common libsoqt4-dev libswscale-dev libswscale-dev libvorbis-dev libx264-dev libxml2-dev libxvidcore-dev

#WORKDIR /root/catkin_ws
#ADD ./utils /root/utils
#ENTRYPOINT [ "/root/utils/entrypoint.sh" ]
#CMD [ "bash" ]

#RUN apt-get update\
#    && apt-get install -y build-essential\
#    && apt-get install -y llvm\
#    && apt-get install -y clang-3.7 libc++-dev libc++abi-dev\
#    && apt-get install -y cmake\
#    && apt-get install -y libopenblas-dev liblapack-dev\
#    && apt-get install -y git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev\
#    && apt-get install -y python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev checkinstall\
#    && apt-get install -y freeglut3-dev libglew*-dev

WORKDIR /root/
RUN git clone https://github.com/rdiankov/collada-dom.git \
    && cd collada-dom \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make -j$(nproc) \
    && make install

#OpenSceneGraph
WORKDIR /root/
RUN apt-get update \
    && apt-get install -y libcairo2-dev libjasper-dev libpoppler-glib-dev libsdl2-dev libtiff5-dev libxrandr-dev \
    && git clone --branch OpenSceneGraph-3.4 https://github.com/openscenegraph/OpenSceneGraph.git \
    && cd OpenSceneGraph \
    && mkdir build \
    && cd build \
    && cmake .. -DDESIRED_QT_VERSION=4 \
    && make -j$(nproc) \
    && make install

#Flexible Collision Library
WORKDIR /root/
RUN apt-get update \
    && apt-get install -y libccd-dev \
    && git clone https://github.com/flexible-collision-library/fcl.git \
    && cd fcl \
    && git checkout 0.5.0 \  
    && mkdir build \ 
    && cd build \
    && cmake .. \
    && make -j$(nproc) \
    && make install

RUN ln -sf /usr/include/eigen3/Eigen /usr/include/Eigen

#Build OpenRAVE    
WORKDIR /root/
RUN git clone --branch latest_stable https://github.com/rdiankov/openrave.git \
    && cd openrave \
    && git checkout 9c79ea260e1c009b0a6f7c03ec34f59629ccbe2c \
    && mkdir build \
    && cd build \
    && cmake .. -DOSG_DIR=/usr/local/lib64/ \
    && make -j$(nproc) \
    && make install

RUN export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(openrave-config --python-dir)/openravepy/_openravepy_ \
    && export PYTHONPATH=$PYTHONPATH:$(openrave-config --python-dir)

#setup python scipy
#WORKDIR /root/
#RUN apt-get install -y libatlas-base-dev gfortran\
#    && apt-get install -y python-scipy
#     && pip install --upgrade matplotlib

#install lsusb
#WORKDIR /root/
#RUN  apt-get install -y usbutils
