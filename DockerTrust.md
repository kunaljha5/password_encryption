# How to Add the Docker-registry cert into Host OS Truststore

%%download the cert from nexus

```
echo | openssl s_client -connect <registry-url>:5000 -showcerts \
| openssl x509 -outform PEM | \
sudo tee -a /etc/pki/ca-trust/source/anchors/registry-url.crt

 
%%update the system level ca certs
sudo update-ca-trust
 
%%restart the docker service
%% sudo service docker restart
sudo systemctl restart docker-latest
```
