## Secure cryptographic materials storage

mkdir -p ~/projects/gostvpn-lab/security
cd ~/projects/gostvpn-lab/security
sudo mkdir -p /security/{certs,private,csr}
sudo chmod 700 /security/private

## Generating CA key and cerfificate

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl genpkey -engine gost -algorithm gost2012_256 -pkeyopt paramset:A -out /security/private/ca.key
sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl req -engine gost -new -x509 -days 3650 -key /security/private/ca.key -out /security/certs/ca.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=GOSTVPN/CN=GOST Root CA"
