# How to Add the Nexus cert into Host OS Truststore

%%download the cert from nexus

```
echo | openssl s_client -connect nexus.aws.netspend.net:5000 -showcerts \
| openssl x509 -outform PEM | \
sudo tee -a /etc/pki/ca-trust/source/anchors/nexus.aws.netspend.net.crt

 
%%update the system level ca certs
sudo update-ca-trust
 
%%restart the docker service
%% sudo service docker restart
sudo systemctl restart docker-latest
```
