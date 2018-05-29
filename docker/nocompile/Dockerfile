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

ADD ./bin ${TERRAINRL_PATH}
ADD ./lib ${TERRAINRL_PATH}/lib/

RUN cd simAdapter && \
    chmod u+x gen_swig.sh && \
    ./gen_swig.sh

RUN pip3 install matplotlib dill && \
    pip3 install --user -v -e ${TERRAINRL_PATH}

### build: docker build -t terrainrlnocompile .
### usage: docker run -it --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" terrainrlnocompile
### test: python3 simAdapter/terrainRLSimTest.py
### test: ./TerrainRL -arg_file= args/biped3D/test_biped_3d_args.txt