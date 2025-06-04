## GOST engine

Cloned into: 'build/gost-engine'

Command used: git clone https://github.com/gost-engine/engine.git build/gost-engine

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
