## Setting up OpenVPN 2.5.9 and OpenSSL 1.1.1u with GOST engine support on a VPS

apt update
apt install git libssl-dev

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

## Creating server configuration file

nano ~/projects/gostvpn-lab/server.ovpn

Server file contents:

port 1194
proto tcp4
dev tun
keepalive 10 120
ca /etc/openvpn/security/certs/ca.crt
cert /etc/openvpn/security/certs/server.crt
key /etc/openvpn/security/private/server.key
auth md_gost12_256
cipher grasshopper-cbc
data-ciphers grasshopper-cbc
tls-version-min 1.2
tls-cipher GOST2012-GOST8912-GOST8912
tls-auth /etc/openvpn/security/private/ta.key 0
tls-server
dh /etc/openvpn/security/certs/dh.pem
topology subnet
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 77.88.8.8"
server 10.8.0.0 255.255.255.0
persist-key
persist-tun
verb 3

## Creating client configuration file

nano ~/projects/gostvpn-lab/client.ovpn

## Copying configuration files over to the VPS

scp ~/projects/gostvpn-lab/server.ovpn ~/projects/gostvpn-lab/client.ovpn root@[VPS IP]:~/projects/gostvpn-lab/

## Moving server configuration file on VPS for consistency

mv ~/projects/gostvpn-lab/server.ovpn /etc/openvpn

## Editing OpenSSL configuration file

nano /usr/local/openssl-gost/ssl/openssl.cnf

Paste at the top of the file:

openssl_conf = openssl_def

[openssl_def]
engines = engine section

[engine_section]
gost = gost_section

[gost_section]
engine_id = gost
default_algorithms = ALL
init = 1

## Exporting OpenSSL configuration file in ~/.bashrc

In ~/.bashrc:
export OPENSSL_CONF=/usr/local/openssl-gost/ssl/openssl.cnf

## Starting OpenVPN server

/usr/local/openvpn-gost/sbin/openvpn --config /etc/openvpn/server.ovpn
