# Docker installation

```bash
yay -S docker
# configure docker
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
# logout, login and then configure docker swarm
docker swarm init
```

# X509 certificates management

Show local / remote PEM certificate details
```bash
# local file
openssl x509 -inform pem -noout -text -in cert.pem
# remote host
echo | openssl s_client -showcerts -servername <host> -connect <host>:<port> 2>/dev/null | openssl x509 -inform pem -noout -text
```

Convert private key and certificate from PEM into JKS format
```bash
cat key.pem cert.pem > key-cert.pem
# enter new export password: changeit
openssl pkcs12 -export -in key-cert.pem -out key-cert.p12
# provide source keystore password: changeit
# enter new destination keystore password: changeit
keytool -importkeystore -srckeystore key-cert.p12 -srcstoretype pkcs12 -destkeystore key-cert.jks
```

Show JKS details
```bash
keytool -v -list -keystore key-cert.jks
```

Import certificate into cacerts
```bash
keytool -import -trustcacerts -keystore $JAVA_HOME/jre/lib/security/cacerts -storepass <changeit> -noprompt -alias <alias> -file cert.pem
```
