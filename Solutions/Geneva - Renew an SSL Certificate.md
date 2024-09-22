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

Then, i's necessary to create a new SLL certificate using openssl. To skip the step where we would have to enter all the necessary information (Company name, State, Country, email, etc.) we'll use the existing certificate to extract that information.

This command creates a self-signed certificate (`nginx.crt.new`) from an existing private key (`nginx.key`) and (`nginx.csr`):
``` bash
openssl x509 \
       -signkey nginx.key \
       -in nginx.csr \
       -req -days 365 -out nginx.crt.new
```


