## GOST engine

Cloned into: 'build/gost-engine'

Command used: git clone -b openssl_1_1_1 https://github.com/gost-engine/engine.git ~/projects/gostvpn-lab/build/gost-engine

## OpenSSL 1.1.1u

Downloaded into: 'build/openssl-1.1.1'

Commands used:
cd build/openssl-1.1.1
wget https://www.openssl.org/source/openssl-1.1.1u.tar.gz
tar -xf openssl-1.1.1u.tar.gz 

## Building OpenSSL 1.1.1u with GOST support

Dependencies installed:
sudo apt update
sudo apt install build-essential zlib1g-dev perl make pkg-config

Build process:
cd ~/projects/gostvpn-lab/build/openssl-1.1.1/openssl-1.1.1u
./config enable-gost --prefix=/usr/local/openssl-gost
make -j$(nproc)
sudo make install

## Environment setup after OpenSSL installation

Environment variables exported in ~/.bashrc:
export PATH=/usr/local/openssl-gost/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/openssl-gost/lib:$LD_LIBRARY_PATH

## Upgrading CMake (3.18+ required)

cd ~/projects/gostvpn-lab/build
wget https://github.com/Kitware/CMake/releases/download/v3.27.9/cmake-3.27.9.tar.gz
tar -xf cmake-3.27.9.tar.gz
cd cmake-3.27.9
./bootstrap --prefix=/usr/local/cmake
make -j$(nproc)
sudo make install

## Environment setup after CMake installation

Environment variable exported in ~/.bashrc:
export PATH=/usr/local/cmake/bin:$PATH

## Building the GOST engine

cd ~/projects/gostvpn-lab/build/gost-engine
mkdir build-cmake && cd build-cmake
cmake .. -DOPENSSL_ROOT_DIR=/usr/local/openssl-gost -DOPENSSL_ENGINES_DIR=/usr/local/openssl-gost/lib/engines-1.1 -DCMAKE_INSTALL_PREFIX=/usr/local/openssl-gost
make -j$(nproc)
sudo make install

## Verifying GOST engine availability

OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 openssl engine -c gost

## Downloading OpenVPN 2.5.9

cd ~/projects/gostvpn-lab/build
wget https://swupdate.openvpn.org/community/releases/openvpn-2.5.9.tar.gz
tar -xf openvpn-2.5.9.tar.gz
cd openvpn-2.5.9
