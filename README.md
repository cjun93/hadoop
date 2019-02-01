# hadoop-odroid
This is for Hadoop 2.9.2 and Odroid XU4 (Ubuntu 18.04 (20180531) (MINIMAL,BARE OS)

# 1. System upgrade
$ sudo apt update
$ sudo apt upgrade && sudo apt autoremove

# 2. Install the required packages
$ sudo apt-get install software-properties-common maven build-essential autoconf automake libtool cmake zlib1g-dev pkg-config libssl-dev bzip2 libbz2-dev libjansson-dev fuse libfuse-dev

## 2.1 libprotobuf-dev installation
This package is for the protobuf-2.5.0
$ sudo apt install libprotobuf-dev

## 2.2 protobuf 2.5.0 compiling and installation
$ wget https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz

$ tar xvzf protobuf-2.5.0.tar.gz

$ cd ~/Sources/protobuf-2.5.0

$ ./autogen.sh

$ ./configure

$ make

$ sudo make install

## 2.3 JAVA jdk 1.8 installation
$ tar xvzf jdk-8u191-linux-arm32-vfp-hflt.tar.gz

$ sudo cp -rp jdk1.8.0_191 /opt/

$ export JAVA_HOME=/opt/jdk1.8.0_191

$ export PATH=$JAVA_HOME/bin:$PATH


# 3. Compile hadoop sources
$ mv hadoop-2.9.2-src.tar.gz ~/Sources

$ cd ~/Sources

$ git clone -b branch-2.9.2 https://github.com/apache/hadoop.git


## 3.1 Patch three files such as HadoopCommon.cmake, OpensslCipher.c and HadoopPipes.cc
$ cd hadoop-2.9.2-src/hadoop-common-project/hadoop-common

$ mv HadoopCommon.cmake HadoopCommon.cmake.bak

$ cp ~/Sources/HadoopCommon.cmake .


$ cd /home/hduser/Sources/hadoop/hadoop-common-project/hadoop-common/src/main/native/src/org/apache/hadoop/crypto

$ mv OpensslCipher.c OpensslCipher.c.bak

$ cp ~/Sources/OpensslCipher.c .


$ cd /home/pi/Sources/hadoop-2.9.2-src/hadoop-tools/hadoop-pipes/src/main/native/pipes/impl

$ mv HadoopPipes.cc HadoopPipes.cc.bak

$ cp ~/Sources/HadoopPipes.cc .

$ cd ~/Sources/hadoop-2.9.2-src/

### Final step
$ mvn package -Pdist,native -DskipTests -Dtar -Dmaven.javadoc.skip=true
