## Setting up OpenVPN 2.5.9 and OpenSSL 1.1.1u with GOST engine support on a VPS

apt update
apt install git

Post-install:
Refer to notes/install-steps.md

## Transferring the /security directory to a VPS

sudo scp -r /security root@[VPS IP]:~

## Copying the /security directory to the standard OpenVPN system path on the VPS

mkdir -p /etc/openvpn/security
mv ~/security/* /etc/openvpn/security

## Enhancing file security permissions

chmod 700 /etc/openvpn/security/private
chmod 600 /etc/openvpn/security/private/*
