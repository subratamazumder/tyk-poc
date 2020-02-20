# tyk-poc
## Installtion
https://github.com/TykTechnologies/tyk-gateway-docker

 2514  docker network create tyk
 2515  docker network ls
 2516  docker pull redis:4.0-alpine
 2517  docker run -itd --rm --name redis --network tyk -p 127.0.0.1:6379:6379 redis:4.0-alpine
 2519  docker pull tykio/tyk-gateway:latest
 2526  git clone https://github.com/TykTechnologies/tyk-gateway-docker.git
 2527  cd tyk-gateway-docker
 2530  docker run -d \\n  --name tyk_gateway \\n  --network tyk \\n  -p 8080:8080 \\n  -v $(pwd)/tyk.standalone.conf:/opt/tyk-gateway/tyk.conf \\n  -v $(pwd)/apps:/opt/tyk-gateway/apps \\n  tykio/tyk-gateway:latest
 2531  docker ps
 2532  curl http://localhost:8080/hello -i
 2533  docker ps
 2534  docker logs --follow tyk_gateway
    

## Management API 
https://tyk.io/docs/tyk-rest-api/

curl -X POST http://localhost:8080/tyk/apis/ -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7' -H 'Content-Type: application/json' -d @keyless.json

curl -X POST http://localhost:8080/tyk/reload/ -H 'x-tyk-authorization: 352d20ee67be67f6340b4c0605b044b7'

curl http://localhost:8080/httpbin/get

 ## Clean up
 2558  docker ps
 2559  docker container ls
 2560  docker container stop tyk_gateway
 2561  docker container stop redis
 2562  docker ps
 2563  docker container ls

## Notes
Sample API JSON https://gist.githubusercontent.com/asoorm/d3adc8b885f8f67a702e2f3789af963d/raw/d534c845c66b3c2a063bd5d1458ca4d10e397bc5/keyless.json
https://tyk.io/docs/basic-config-and-security/security/tls-and-ssl/mutual-tls/
https://tyk.io/docs/basic-config-and-security/security/certificate-pinning/
