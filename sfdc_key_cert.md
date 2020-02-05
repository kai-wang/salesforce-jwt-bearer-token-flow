openssl genrsa -des3 -passout pass:SomePassword -out server.pass.key 2048

openssl rsa -passin pass:SomePassword -in server.pass.key -out server.key

rm -rf server.pass.key

openssl req -new -key server.key -out server.csr

openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt

* Now private key and cert are generated
* Import the cert server.crt to Salesforce target server connected App
* To test the JWT connection from node.js, you can use the server.key
* iss: comsumer_key, aus: https://login.salesforce.com, sub: salesforce login user name


##### To use Salesforce named credential from source org, use the below commands to generate the JKS file

openssl pkcs12 -export -in server.crt -inkey server.key -out abc.p12
keytool -importkeystore -srckeystore abc.p12 -srcstoretype PKCS12 -destkeystore abc.jks -deststoretype JKS
##### please use the below command to check the alias name, if not correct, you can change it
keytool -list -v -keystore ./abc.jks

keytool -keystore abc.jks -changealias -alias 1 -destalias test

keytool -list -v -keystore ./abc.jks

### Rerefences
https://help.salesforce.com/articleView?id=000338348&language=en_US&type=1&mode=1
https://stackoverflow.com/questions/11952274/how-can-i-create-keystore-from-an-existing-certificate-abc-crt-and-abc-key-fil
https://stackoverflow.com/questions/17695297/importing-the-private-key-public-certificate-pair-in-the-java-keystore
