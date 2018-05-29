FROM ubuntu:18.04
# MAINTAINER <name> "<email>"

ENV TERRAINRL_PATH /opt/TerrainRLSim
ENV NCORES 2
ENV DEBIAN_FRONTEND noninteractive

WORKDIR ${TERRAINRL_PATH}

RUN apt-get -qq update && apt-get -y dist-upgrade && \
    apt-get install -y git wget python3-tk \
                    libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler \
                    libgflags-dev libgoogle-glog-dev liblmdb-dev \
                    libatlas-base-dev \
                    gcc-multilib g++-multilib \
                    libf2c2-dev \
                    libglew-dev freeglut3 freeglut3-dev \
                    premake4 \
                    swig3.0 python3-dev python3-pip \
                    mesa-utils && \
    apt-get install --no-install-recommends libboost-all-dev -y && \
    cd /usr/lib/x86_64-linux-gnu/ && \
    ln -sf libhdf5_serial_hl.so libhdf5_hl.so && \
    ln -sf libhdf5_serial.so libhdf5.so

RUN cd /opt && \
    git clone https://github.com/UBCMOCCA/TerrainRLSim.git && \
    cd TerrainRLSim && \
    mkdir output

#############################################################
######################## START BUILD ########################
RUN wget https://github.com/xbpeng/DeepLoco/releases/download/0.81/TerrainRL_external_Linux_June_2017.tar.gz && \
    tar -xf TerrainRL_external_Linux_June_2017.tar.gz && \
    rm TerrainRL_external_Linux_June_2017.tar.gz && \
    cd external/caffe && \
    make clean && \
    make OPENCV_VERSION=3 -j ${NCORES}

RUN cp -r external/caffe/build/lib . && \
    cp external/caffe/build/lib/libcaffe.* lib/ && \
    cp external/Bullet/bin/*.so lib/ && \
    cp external/jsoncpp/build/debug/src/lib_json/*.so* lib/ && \
    cd simAdapter && \
    chmod u+x gen_swig.sh && \
    ./gen_swig.sh && \
    cd .. && \
    chmod u+x premake4_linux && \
    ./premake4_linux clean && \
    ./premake4_linux gmake

RUN cd gmake && \
    make -j ${NCORES} config=release64 && \
    cd .. &&\
    cp x64/Release/* .
######################## END BUILD ########################
###########################################################

RUN pip3 install matplotlib dill && \
    pip3 install --user -v -e ${TERRAINRL_PATH}

### build: docker build -t terrainrlnodata .
### usage: docker run -it --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" terrainrlnodata
### test: python3 simAdapter/terrainRLSimTest.py
### test: ./TerrainRL -arg_file= args/biped3D/test_biped_3d_args.txt