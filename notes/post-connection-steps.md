## Enabling IP forwarding on the VPS

nano /etc/sysctl.conf

Uncomment the line: net.ipv4.ip_forward = 1

sysctl -p

## Applying NAT rule

iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o ens3 -j MASQUERADE

## Making NAT rule permanent

wget http://ftp.uk.debian.org/debian/pool/main/i/iptables-persistent/netfilter-persistent_1.0.23_all.deb
dpkg -i netfilter-persistent_1.0.23_all.deb
netfilter-persistent save

## Testing 

Initiate client connection with server, test client IP with: curl ifconfig.me

Server and client IP should match.
