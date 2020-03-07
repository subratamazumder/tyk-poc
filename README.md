# tyk-poc Mutual TLS & Certificate Pinning
## Installtion
https://github.com/TykTechnologies/tyk-gateway-docker

```console
docker network create tyk

docker network ls

docker pull redis:4.0-alpine

docker run -itd --rm --name redis --network tyk -p 127.0.0.1:6379:6379 redis:4.0-alpine

docker pull mongo

docker run -itd --rm --name mongo --network tyk -p 127.0.0.1:27017:27017 mongo:latest

docker pull tykio/tyk-gateway:latest

git clone https://github.com/TykTechnologies/tyk-gateway-docker.git

cd tyk-gateway-docker [need addtional tweak for dashboard compatibility]

docker run -itd --rm --name tyk-gateway --network tyk -p 8080:8080 -v $(pwd)/tyk.standalone.conf:/opt/tyk-gateway/tyk.conf -v $(pwd)/apps:/opt/tyk-gateway/apps tykio/tyk-gateway:latest

test community version > curl http://localhost:8080/hello -i

docker run -itd --rm --name tyk-dashboard -p 3000:3000 --link redis --link mongo --link tyk-gateway tykio/tyk-dashboard

docker run -it --rm --name tyk-dashboard --network tyk -p 3000:3000 --link redis --link mongo --link tyk-gateway tykio/tyk-dashboard:latest

test dashboard version> http://dashboard.tyk.docker:3000/ [need license to use]


docker ps

docker logs --follow tyk_gateway
    
```

## Working with Management API 
https://tyk.io/docs/tyk-rest-api/

#### Deploy API
```console

curl -X POST http://localhost:8080/tyk/apis/ -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7' -H 'Content-Type: application/json' -d @keyless.json

curl -X GET http://localhost:8080/tyk/reload/ -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7'

curl -X PUT http://localhost:8080/tyk/apis/3 -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7' -H 'Content-Type: application/json' -d @api-mtls.json
{"key":"3","status":"ok","action":"modified"}
```
#### Verify API
```console
curl http://localhost:8080/httpbin/get

```

#### Upload Certificate

```console
curl -X POST \
  http://localhost:8080/tyk/certs \
  -H 'Postman-Token: b08dd295-b733-43fd-9443-810107bc0657' \
  -H 'cache-control: no-cache' \
  -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7' \
  -d '-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC4bFDbVC58zdHN
/vhPwWVXwpK0R6O4CT+DbH5gputlfCTHxt8Er8DIwT/m7V1uX6pBHhjCnXa4Iao6
Io1V1t25ZqbeCQ1y2dTcxjbJ/oBVo+4yn8mUvogB1IoiaYWwJpg4Uuz7n0AbkrJj
sWjN8OFtNtPCD4sfS56uWbV0uKCTRnSkQFa4UvtWRj36kHP/YSi7iPxduvnXqC9k
FIIuC+T+j2IbU+HYVz82skZnOo+Tb3ZrUsLwvIPnsNJBZdEi5lUldwcde/LIQU2h
0YKXjqTEPdAmYUhOLss8KKWd5enUFWV9dzqgYTsh4t/GruUuLhaUSL1FJMkaKZYT
8N5S1m+7AgMBAAECggEAGHqwJNzAquo67gfo99Uo2YRHKszTi2sW1iABily9pCPd
UfMwyRN3GG6mR8W8ABmMpMYU7UgvaPN2/+50Ki+yEJjyj0hOU69cVM2umhNA/50Z
0fhprme3795BU54EE8SbseY39JJH1SEcsqTqz2Mo2PFNHFYp1kaYUnYv4sVa8xFO
L4YQBSHul6xMJ0aXPvOpjhaWE5EtmThAh9Q/8K/2+BzoD4lJg317uZsLVoawt4M7
hv134ei/XK/lodvYJCeD7aex6vqaDlA6WSB/mwIZcKuxouTA0AF+dUlD0c4OqeGG
eqPwUha9PUNrYvs4GLQAvTqj4gBFSGnsa0bmpaOvWQKBgQDrGg40ClbY9aVkhBIA
MZ8CYH9Delximp8vGFDXeybMaRrSfP2G/kKg4FXo2CT1VjN7qgCRkrjegJ+MhMcI
YNymVOLj9VauqHLzAxZbU9BW/B8yE/ghkNSlDaxizP52z7o0NmcHba+HXUJhTXZt
Qr5u0uEPEbiVXvOFpd8RfDy/7QKBgQDI0QX5+VpnBEjN0lkuKNfD1fUWfwonsSpD
TXpHSmWA4HF3RJlczPwgpRbbSnS1dGofW52fTTirKGf9QtwdEjYtNLTBweysexXg
KCrfWwAucmtbwHhJS8cjqD1tfEzgSZiojA+r4loka8Mr2v6FZnl4sTVED+1bLMy+
b4+cJblpRwKBgQCcRFhWfNzXHwgNNL/mQxVG9i8BAg7wN4hBPG9XmvLiAaajbBL0
LILK/fH8b9a4/8/+jbQNDrI0qtfiBctppUBkip25GbTBKRQmtNGiaKZdev2dQqq2
XNcK0njXvxwQiuhglhyLUnvOhM1/cYaa/zcm4KJZatT+6/r/xY3syGB0zQKBgBEn
OZl2oTA3f3iFRTTaLEQAHKVFSLrHOVLyZUV9p1nw0gBcDbWNlOO89kzY+UsenIn9
K2OWFwcXtno9ocuh6JrH68C5Ldw1z1KMW80kWsmc4Gq/8AZiwKndDxIxEef+oVmU
TSpjdUuzIMK4PBFtBWc3y9L6gU3Ob9a8dMnjtwi1AoGBANagiUVvRezXQplqXgv5
wtXjTyb6SKbbhI5UAMZ/gotr1AJqhARKatO5KmTlpmZsnPviZqJ487SlLsvJO9AZ
Vr3YnyjHIGoVM7T6qp8iX3dkzMX3whFGWVcFb83Nk/QAosHucAsVRAfJgHAZ+/AZ
wGWR3RPTnsrmmspFRoJ1FwaF
-----END PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIDgDCCAmgCCQCXMJb2RAyThTANBgkqhkiG9w0BAQsFADCBgTELMAkGA1UEBhMC
SU4xEjAQBgNVBAgMCUJhbmdhbG9yZTEOMAwGA1UEBwwFRUNpdHkxGzAZBgNVBAoM
ElN1YnJhdGEgUE9DIENsaW5ldDEMMAoGA1UECwwDUE9DMSMwIQYDVQQDDBprdWJl
cm5ldGVzLmRvY2tlci5pbnRlcm5hbDAeFw0yMDAyMjIyMjU0MjdaFw0yMTAyMjEy
MjU0MjdaMIGBMQswCQYDVQQGEwJJTjESMBAGA1UECAwJQmFuZ2Fsb3JlMQ4wDAYD
VQQHDAVFQ2l0eTEbMBkGA1UECgwSU3VicmF0YSBQT0MgQ2xpbmV0MQwwCgYDVQQL
DANQT0MxIzAhBgNVBAMMGmt1YmVybmV0ZXMuZG9ja2VyLmludGVybmFsMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuGxQ21QufM3Rzf74T8FlV8KStEej
uAk/g2x+YKbrZXwkx8bfBK/AyME/5u1dbl+qQR4Ywp12uCGqOiKNVdbduWam3gkN
ctnU3MY2yf6AVaPuMp/JlL6IAdSKImmFsCaYOFLs+59AG5KyY7FozfDhbTbTwg+L
H0uerlm1dLigk0Z0pEBWuFL7VkY9+pBz/2Eou4j8Xbr516gvZBSCLgvk/o9iG1Ph
2Fc/NrJGZzqPk292a1LC8LyD57DSQWXRIuZVJXcHHXvyyEFNodGCl46kxD3QJmFI
Ti7LPCilneXp1BVlfXc6oGE7IeLfxq7lLi4WlEi9RSTJGimWE/DeUtZvuwIDAQAB
MA0GCSqGSIb3DQEBCwUAA4IBAQCxR/2j3D3VwfiDCqKAWr9lBmDRQgKkoHym6Ss5
hr8yzvcGIZjbaJh9RW41NrfgEMMxKS+DnhyBW+OOyZfURgY6vuRcU0vbi2JyeCYh
LwaS7HrJRejTw2+NZkBYmHSXdrkpNlbsjc0mSvpJcIS/yo3FD2csvnNSmO8Js603
fxMkYe4aI//pWtcSTgfqwWAgUMAlwyE8AGucGU4Ujvyp0tow5q0LRQWfb/xqY7lZ
X5+2h4TU+uP/Drrbq1dryiwR2BJTMiFTBWL8yFC+BAiod+4P0jI2zs03c4WbSm5P
Mz4FrUiobXPLy5fqinHuzqSKtsou9/2nsR2TNAH9MLvhiY+7
-----END CERTIFICATE-----'
```
#### Verify Certificate
```console
Subratas-MacBook-Pro  ~/workspace/nginx-poc/mtls   master  curl -X GET http://localhost:8080/tyk/certs/ -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7'
{"certs":["81161276e7801307424120ac929503bf9da703cd5462adb8bd4d361354131a49"]}
 subratas-MacBook-Pro  ~/workspace/nginx-poc/mtls   master  
 
 
curl -X DELETE http://localhost:8080/tyk/certs/81161276e7801307424120ac929503bf9da703cd5462adb8bd4d361354131a49 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7'
 curl -X GET http://localhost:8080/tyk/certs/81161276e7801307424120ac929503bf9da703cd5462adb8bd4d361354131a49 -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7'
{"id":"81161276e7801307424120ac929503bf9da703cd5462adb8bd4d361354131a49","fingerprint":"81161276e7801307424120ac929503bf9da703cd5462adb8bd4d361354131a49","has_private":true,"issuer":{"Country":["IN"],"Organization":["Subrata POC Clinet"],"OrganizationalUnit":["POC"],"Locality":["ECity"],"Province":["Bangalore"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"subratapocclient","Names":[{"Type":[2,5,4,6],"Value":"IN"},{"Type":[2,5,4,8],"Value":"Bangalore"},{"Type":[2,5,4,7],"Value":"ECity"},{"Type":[2,5,4,10],"Value":"Subrata POC Clinet"},{"Type":[2,5,4,11],"Value":"POC"},{"Type":[2,5,4,3],"Value":"subratapocclient"}],"ExtraNames":null},"subject":{"Country":["IN"],"Organization":["Subrata POC Clinet"],"OrganizationalUnit":["POC"],"Locality":["ECity"],"Province":["Bangalore"],"StreetAddress":null,"PostalCode":null,"SerialNumber":"","CommonName":"subratapocclient","Names":[{"Type":[2,5,4,6],"Value":"IN"},{"Type":[2,5,4,8],"Value":"Bangalore"},{"Type":[2,5,4,7],"Value":"ECity"},{"Type":[2,5,4,10],"Value":"Subrata POC Clinet"},{"Type":[2,5,4,11],"Value":"POC"},{"Type":[2,5,4,3],"Value":"subratapocclient"}],"ExtraNames":null},"not_before":"2020-02-22T12:47:19Z","not_after":"2021-02-21T12:47:19Z"}
```

#### Update API with Certificate ID
```console
curl -X PUT http://localhost:8080/tyk/apis/2 -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7' -H 'Content-Type: application/json' -d @api-mtls.json
{"key":"2","status":"ok","action":"modified"}
```
#### Verify API
```console
curl http://localhost:8080/tykmtls2/

```
 
 ## Clean up
 ```console
 docker container stop $(docker ps -aq)
 
 docker ps

 docker container ls

 docker container stop tyk_gateway

 docker container stop redis

 docker ps

 docker container ls
```

## Notes
Sample API JSON https://gist.githubusercontent.com/asoorm/d3adc8b885f8f67a702e2f3789af963d/raw/d534c845c66b3c2a063bd5d1458ca4d10e397bc5/keyless.json

https://tyk.io/docs/basic-config-and-security/security/tls-and-ssl/mutual-tls/

https://tyk.io/docs/basic-config-and-security/security/certificate-pinning/

https://tyk.io/docs/tyk-configuration-reference/tyk-gateway-configuration-options/

https://github.com/TykTechnologies/tyk-gateway-docker

https://github.com/TykTechnologies/tyk-dashboard-docker


sudo openssl req -x509 -nodes -days 365 -subj '/C=IN/ST=Bangalore/L=ECity/O=Subrata POC Clinet/OU=POC/CN=kubernetes.docker.internal' -newkey rsa:2048 -keyout nginx-client.key -out nginx-client.crt

## Errors Encountered
Only client cert has CN as domain name
```
[Feb 22 23:03:00] DEBUG Started proxy
[Feb 22 23:03:00] DEBUG Stripping: /tykmtls1/
[Feb 22 23:03:00] DEBUG Upstream Path is: 
[Feb 22 23:03:00] DEBUG Started api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy org_id=default ts=1582412580858985100
[Feb 22 23:03:00] DEBUG Upstream request URL:  api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy org_id=default
[Feb 22 23:03:00] DEBUG Outbound request URL: https://kubernetes.docker.internal:8134/sample.html api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy org_id=default
[Feb 22 23:03:00] DEBUG Found upstream mutual TLS certificate api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy org_id=default
[Feb 22 23:03:00] ERROR proxy: http: proxy error: x509: certificate is valid for subratapoc, not kubernetes.docker.internal api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy org_id=default server_name=kubernetes.docker.internal:8134 user_id=-- user_ip=172.18.0.1 user_name=
[Feb 22 23:03:00] DEBUG Adding Healthcheck to: 3.BlockedRequest
[Feb 22 23:03:00] DEBUG Val is: -1
[Feb 22 23:03:00] DEBUG Finished api_id=3 api_name=Tyk mtls 1 mw=ReverseProxy ns=11206100 org_id=default
[Feb 22 23:03:00] DEBUG Upstream request took (ms): 11.258
[Feb 22 23:03:00] DEBUG Done proxy
```

After chnaging server cert to CN as domain name
```
[Feb 22 23:49:27] DEBUG Started proxy
[Feb 22 23:49:27] DEBUG Stripping: /tykmtls2/
[Feb 22 23:49:27] DEBUG Upstream Path is: 
[Feb 22 23:49:27] DEBUG Started api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy org_id=default ts=1582415367521383200
[Feb 22 23:49:27] DEBUG Upstream request URL:  api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy org_id=default
[Feb 22 23:49:27] DEBUG Outbound request URL: https://kubernetes.docker.internal:8134/sample.html api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy org_id=default
[Feb 22 23:49:27] DEBUG Found upstream mutual TLS certificate api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy org_id=default
[Feb 22 23:49:27] ERROR proxy: http: proxy error: x509: certificate signed by unknown authority api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy org_id=default server_name=kubernetes.docker.internal:8134 user_id=-- user_ip=172.18.0.1 user_name=
[Feb 22 23:49:27] DEBUG Adding Healthcheck to: 4.BlockedRequest
[Feb 22 23:49:27] DEBUG Val is: -1
[Feb 22 23:49:27] DEBUG Finished api_id=4 api_name=Tyk mtls 2 mw=ReverseProxy ns=21307800 org_id=default
[Feb 22 23:49:27] DEBUG Upstream request took (ms): 22.0134
[Feb 22 23:49:27] DEBUG Done proxy
```
 
