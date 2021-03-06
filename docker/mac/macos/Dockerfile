FROM ubuntu:18.04 
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y keyboard-configuration
RUN DEBIAN_FRONTEND=noninteractive apt install -y tzdata
RUN apt install -y build-essential libtool autotools-dev automake pkg-config bsdmainutils curl git
RUN apt-get install -y libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
RUN apt-get install -y libdb4.8 libdb4.8++
RUN apt-get install -y libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev python3 libboost-test-dev libboost-thread-dev
RUN apt-get install -y libzmq3-dev wget
RUN apt-get install -y libminiupnpc-dev
RUN apt-get install -y libtool
RUN apt-get install -y libcurl4-openssl-dev
RUN apt-get install libgmp3-dev libevent-dev bsdmainutils libboost-all-dev openssl -y
RUN apt-get install -y python-pip
RUN apt-get install -y cmake
RUN apt-get install -y libcap-dev
RUN apt-get install -y python-dev
RUN apt-get install -y libbz2-dev
RUN apt-get install -y imagemagick
RUN PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')

# CLONING REPO
WORKDIR /opt
RUN git clone https://github.com/scryptachain/scrypta

# BUILDING LINUX
WORKDIR /opt/scrypta/depends
RUN mkdir SDKs
WORKDIR /opt/scrypta/depends/SDKs

RUN pip install setuptools
RUN wget -c https://github.com/phracker/MacOSX-SDKs/releases/download/MacOSX10.11.sdk/MacOSX10.11.sdk.tar.xz
RUN tar -xf MacOSX10.11.sdk.tar.xz
WORKDIR /opt/scrypta/depends
RUN make HOST=x86_64-apple-darwin11
WORKDIR /opt/scrypta

RUN ./autogen.sh
RUN mkdir db4
RUN wget -c 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
RUN tar -xzvf db-4.8.30.NC.tar.gz
WORKDIR /opt/scrypta/db-4.8.30.NC/build_unix/
RUN ../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$(pwd)/db4
RUN make install
WORKDIR /opt/scrypta
RUN ./autogen.sh
RUN ./configure --prefix=$(pwd)/depends/x86_64-apple-darwin11
RUN make
RUN make deploy

RUN make
WORKDIR /opt/scrypta
RUN ./autogen.sh 
RUN ./configure --prefix=$(pwd)/depends/x86_64-pc-linux-gnu
RUN make

EXPOSE 42222 42223
CMD tail -f /dev/null