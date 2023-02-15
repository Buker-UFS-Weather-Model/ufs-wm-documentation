Clone the repository

```console
git clone https://github.com/ufs-community/ufs-weather-model.git ufs-weather-model
cd ufs-weather-model
git checkout ufs-v1.1.0
git submodule update --init --recursive
```

Install or update CMake.

```console
sudo apt-get update
sudo apt install cmake
cmake --version
```

install or update C++ compiler

```console
sudo apt install gcc
```

install or update Fortran compiler
```console
sudo apt install gfortran
```

SHOULD have a way to check defualt compilers for CMake

Install or update MPI library. Not sure if this is working?

```console
sudo apt install mpi
```

follow instructions [here](https://github.com/NOAA-EMC/NCEPLIBS-external/blob/develop/doc/README_redhat_gnu.txt) to prepare build
``` console
# Install compilers and utilities 
sudo su
# Optional: this is usually a good idea, but should be used with care (it may destroy your existing setup)
apt update
# Install wget-1.19.5
apt install -y wget
# Install git-2.18.2
apt install -y git
# Install make-4.2.1
apt install -y make
# Install openssl-devel-1.1.1
apt install -y openssl-devel
# Install patch-2.7.6
apt install -y patch
# Install Python-3.8.0
apt install -y python38
alternatives --config python
# choose /usr/bin/python3
# Install libxml2-2.9.7 (for xmllint)
apt install libxml2
# Install m4-1.4.18
apt install m4
# Install gcc-9.2.1 - does this work w/o installing gcc-8.3.1 above?
apt install gcc-toolset-9-gcc-c++.x86_64 gcc-toolset-9-gcc-gfortran.x86_64
```
```
mkdir ufs-develop
chown -R ec2-user:ec2-user ufs-develop
exit

#scl enable gcc-toolset-9 bash

export CC=gcc
export CXX=g++
export FC=gfortran

cd ufs-develop
mkdir src
```
```console
# install missing libraries from NCEPLIBS-external

cd ${UFS_DEVELOP_PREFIX}/src
git clone -b develop --recursive https://github.com/NOAA-EMC/NCEPLIBS-external
cd NCEPLIBS-external
# Install cmake 3.16.3 (default OS version is too old)
cd cmake-src
./bootstrap --prefix=${UFS_DEVELOP_PREFIX} 2>&1 | tee log.bootstrap
make 2>&1 | tee log.make
make install 2>&1 | tee log.install
cd ..
mkdir build && cd build
${UFS_DEVELOP_PREFIX}/bin/cmake -DCMAKE_INSTALL_PREFIX=${UFS_DEVELOP_PREFIX} .. 2>&1 | tee log.cmake
make -j8 2>&1 | tee log.make
# no make install needed
```



```console
cd ${UFS_DEVELOP_PREFIX}/src

git clone -b develop --recursive https://github.com/NOAA-EMC/NCEPLIBS

cd NCEPLIBS
mkdir build
cd build

${UFS_DEVELOP_PREFIX}/bin/cmake -DCMAKE_INSTALL_PREFIX=${UFS_DEVELOP_PREFIX} -DCMAKE_PREFIX_PATH=${UFS_DEVELOP_PREFIX} .. 2>&1 | tee log.

cmake

make -j8 2>&1 | tee log.make
# no make install needed
```