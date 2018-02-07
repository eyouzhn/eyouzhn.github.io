---
layout:     post
title:      如何生成Client,server和PEM证书  
subtitle:   自动生成
date:       2018-02-07
author:     BY Jeff Zhan
header-img: img/post-bg-BJJ.jpg
catalog: true
tags:
    - Test
---
```
www:/opt/miep/lib/node_modules/backendSimulator/config/Certificate # vi generate.sh   
# genereate server private key
openssl genrsa -out server.key 1024
# genereate server public key
openssl rsa -in server.key -pubout -out server.pem
# genereate client private key
openssl genrsa -out client.key 1024
# generate client public key
openssl rsa -in client.key -pubout -out client.pem


# genereate CA private key ?? CA ??
openssl genrsa -out ca.key 1024
# X.509 Certificate Signing Request (CSR) Management.
expect  generate_csr.exp "openssl req -new -key ca.key -out ca.csr" "CA_OR"
# X.509 Certificate Data Management.
openssl x509 -req -days 1000 -in ca.csr -signkey ca.key -out ca.crt


# server need to apply for signed cerficate to CA  
expect generate_csr.exp "openssl req -new -key server.key -out server.csr" "SERVER"
# ???? CA ??????,?????? CA ????????,???????? CA ?????
openssl x509 -req -days 1000 -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt
# client ?
expect  generate_csr.exp "openssl req -new -key client.key -out client.csr" "CLIENT"
# client ?? CA ??
openssl x509 -req -days 1000 -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt

cat ca.crt ca.key > ca.pem

www:/opt/miep/lib/node_modules/backendSimulator/config/Certificate # vi generate_csr.exp   
#!/usr/bin/expect
spawn bash -i
expect "#"

set command [lindex $argv 0]
set ORGANIZATION [lindex $argv 1]
#send "openssl req -new -key ca.key -out ca.csr\r"
send "$command\r"

#expect "Country Name (2 letter code) [AU]:"
expect ":"
send "CN\r"

#expect "State or Province Name (full name) [Some-State]:"
expect ":"
send "GD\r"

#expect "Locality Name (eg, city) []:"
expect ":"
send "GZ\r"

#expect "Organization Name (eg, company) [Internet Widgits Pty Ltd]:"
expect ":"
send "$ORGANIZATION\r"

#expect "Organizational Unit Name (eg, section) []:"
expect ":"
send "SES\r"

#expect "Common Name (e.g. server FQDN or YOUR name) []:"
expect ":"
send "www.dummyserver.com\r"

#expect "Email Address []:"
expect ":"
send "80242319@qq.com\r"

#expect "A challenge password []:"
expect ":"
send "\r"

#expect "An optional company name []:"
expect ":"
send "\r"
expect "#"
www:/opt/miep/lib/node_modules/backendSimulator/config/Certificate # 
```

这样就可以用 sh generate.sh 来自动生成证书了。