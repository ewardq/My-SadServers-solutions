---
dg-publish: true
---
---
**Scenario:** "Geneva": Renew an SSL Certificate
**Level:** Easy
**Type:** Fix  
**Description:** There's an Nginx web server running on this machine, configured to serve a simple "Hello, World!" page over HTTPS. However, the SSL certificate is expired.  
  
Create a new SSL certificate for the Nginx web server with the same Issuer and Subject (same domain and company information).

**Test:** Certificate should not be expired: `echo | openssl s_client -connect localhost:443 2>/dev/null | openssl x509 -noout -dates` and the subject of the certificate should be the same as the original one: `echo | openssl s_client -connect localhost:443 2>/dev/null | openssl x509 -noout -subject`  
  
The "Check My Solution" button runs the script _/home/admin/agent/check.sh_, which you can see and execute

---
## Notes and solution
First, let's see where the current certificate is located and if there is a private key with it.

``` bash
cat /etc/nginx/sites-available/default
```

![[CertLocationNginx.PNG]]
where we can see that both the certification and the private key are stored in _/etc/nginx/ssl/_

Then, it's necessary to create a new SLL certificate using openssl. To skip the step where we would have to enter all the necessary information (Company name, State, Country, email, etc.) we'll use the existing certificate to extract that information.


> [!Important] Important
> `.key` files are usually the private key, used by the server to encrypt and package data for verification by clients.
> `.pem` files are generally the public key, used by the client to verify and decrypt data sent by servers. PEM files could also be encoded private keys, content must be verified before assuming any or the other.
> `.p12` files have both halves of the key embedded, so that administrators can easily manage halves of keys.
> `.cert` or `.crt` files are the signed certificates.
> `.csr` is a certificate signing request, a challenge used by a trusted third party to verify the ownership of a keypair without having direct access to the private key.
> 	- In the self-signed scenario you will use the certificate signing request with your own private key to verify your private key (thus self-signed). Depending on your specific application, this might not be needed. (needed for web servers or RPC servers, but not much else).

Here is an example of what the CSR information prompt will look like:

```bash 
---
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:Brooklyn
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Example Brooklyn Company
Organizational Unit Name (eg, section) []:Technology Division
Common Name (e.g. server FQDN or YOUR name) []:examplebrooklyn.com
Email Address []:
```

To get this information out of the expired SSL certificate, we can use `OpenSSL`.
``` bash
openssl x509 -noout -text -in nginx.crt
```
![[Pasted image 20240923084001.png]]

Here, we can see that the original certificate has the following information:
``` bash
Issuer: CN = localhost, O = Acme, OU = IT Department, L = Geneva, ST = Geneva, C = CH
Subject: CN = localhost, O = Acme, OU = IT Department, L = Geneva, ST = Geneva, C = CH
Validity
            Not Before: Sep 17 22:34:18 2024 GMT
            Not After : Sep 18 22:34:18 2023 GMT
```

To enter this information non-interactively when creating the signing request (`.csr`), use the `-subj` option for OpenSSL.
``` bash
-subj "/C=CH/ST=Geneva/L=Geneva/O=Acme/CN=localhost"
```

This command creates a self-signed certificate (`nginx.crt.new`) from an existing private key (`nginx.key`) and (`nginx.csr`):
``` bash
openssl x509 \
       -signkey nginx.key \
       -in nginx.csr \
       -req -days 365 -out nginx.crt
```


