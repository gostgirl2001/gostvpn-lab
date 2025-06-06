## Secure cryptographic materials storage

mkdir -p ~/projects/gostvpn-lab/security
cd ~/projects/gostvpn-lab/security
sudo mkdir -p /security/{certs,private,csr}
sudo chmod 700 /security/private

## Generating CA key and cerfificate

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl genpkey -engine gost -algorithm gost2012_256 -pkeyopt paramset:A -out /security/private/ca.key
sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl req -engine gost -new -x509 -days 3650 -key /security/private/ca.key -out /security/certs/ca.crt -subj "/C=RU/ST=Moscow/L=Moscow/O=GOSTVPN/CN=GOST Root CA"

## Generating server key and CSR

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl genpkey -engine gost -algorithm gost2012_256 -pkeyopt paramset:A -out /security/private/server.key
sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl req -engine gost -new -key /security/private/server.key -out /security/csr/server.csr -subj "/CN=server"

## Signing server certificate using CA and GOST engine

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl x509 -req -in /security/csr/server.csr -CA /security/certs/ca.crt -CAkey /security/private/ca.key -CAcreateserial -out /security/certs/server.crt -days 3650 -extensions v3_req -extfile /usr/lib/ssl/openssl.cnf -engine gost

## Generating client key and CSR

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl genpkey -engine gost -algorithm gost2012_256 -pkeyopt paramset:A -out /security/private/client.key
sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl req -engine gost -new -key /security/private/client.key -out /security/csr/client.csr -subj "/CN=client"

## Signing client certificate using CA and GOST engine

sudo OPENSSL_ENGINES=/usr/local/openssl-gost/lib/engines-1.1 /usr/local/openssl-gost/bin/openssl x509 -req -in /security/csr/client.csr -CA /security/certs/ca.crt -CAkey /security/private/ca.key -CAcreateserial -out /security/certs/client.crt -days 3650 -extensions v3_req -extfile /usr/lib/ssl/openssl.cnf -engine gost

## Generating Diffie-Hellman parameters

sudo /usr/local/openssl-gost/bin/openssl dhparam -out /security/certs/dh.pem 2048

## Generating TLS key

sudo /usr/local/openvpn-gost/sbin/openvpn --genkey --secret /security/private/ta.key
