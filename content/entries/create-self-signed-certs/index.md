+++
title = "Create Self Signed Certs"
slug = "create-self-signed-certs"
date = 2021-05-16T22:42:39+08:00
lastmod = 2021-05-16T22:42:39+08:00
publishDate = 2021-05-16T22:42:39+08:00
draft = true
+++

# create CA
``` bash
openssl genrsa -des3 -out CA.key 4096                                   # create private CA key
openssl req -x509 -new -nodes -key CA.key -sha256 -days 365 -out CA.crt # create self signed CA
```

# create certifications for domain
``` bash
openssl genrsa -out domain.key 2048
openssl req -new -sha256 -key domain.key -subj "/C=CN/ST=Shanghai/O=MyOrg/CN=domain.com" -out domain.csr
openssl x509 -req -in domain.csr -CA CA.crt -CAkey CA.key -CAcreateserial -out domain.crt -days 365 -sha256
```
