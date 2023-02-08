# Code Server + HAproxy + EasyRSA CA

Code Server is VS code but in the browser. This repository will let you spin it up with Docker Compose and also let you sign it with a CA.

## Setup

### Step 1: Code-Server

Modify .env with the correct values. 

### Step 2: SSL Cert Part 1 - EasyRSA 

Note: You can set FQDN variable in bash to make your life easier ;) 

1. Create CA: `easyrsa init-pki`
2. Build CA" `easyrsa build-ca nopass # set base domain`
3. Create CSR: `easyrsa gen-req $FQDN nopass`
4. Import CSR: `easyrsa import-req pki/reqs/$FQDN.req $FQDN`
5. Sign CSR: `easyrsa sign-req server $FQDN`
6. Create PEM: `cat pki/issued/$FQDN.crt pki/private/$FQDN.key > haproxy/$FQDN.pem`

### Step 3: SSL Cert Part 2 - HAproxy

Change FQDN .env variable to be the domain name. Will look for certificate in pki/issued/$(FQDN).pem.


## Notes

### Port 80 Taken on Windows 

Stop all Windows HTTP services with: 

`NET stop HTTP`
