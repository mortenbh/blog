# CACert.org

Last modified: 20-08-2010 00:53:10

## Using your client certificate with OpenSSH

To use your client certificate with [OpenSSH](http://openssh.org/) you will
first need to export your public-private key pair from your browser in PKCS#12
format. You can then use [OpenSSL](http://openssl.org/) to extract your private
key to PEM format, which OpenSSH uses.

	openssl pkcs12 -in exported.p12 -clcerts -out .ssh/id_rsa

Finally you can use `ssh-keygen` to generate a public key for your private key.

	ssh-keygen -y -f .ssh/id_rsa

For convenience, use `ssh-copy-id` to copy your public key to hosts.

## Renewing server certificate

Simply paste the server certificate from cacert.org into

	/etc/ssl/certs/alas_cert.pem

*[PKCS] Public-Key Cryptography Standards
*[PEM] Privacy Enchanced Mail
