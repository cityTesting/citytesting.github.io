---
layout: post
title: How to test certificates with Wiremock and Postman
categories: [wiremock, postman]
---

When developing an application where certificates are used it is often convenient to test it using a non-real system to verify that we are creating a good request with the certificate included. Therefore we are going to show here how to simulate with wiremock a system which needs a valid certificate to respond correctly and we are going to validate it using Postman.

The steps to follow are as follows:
- Create the certificates:

> keytool  -genkey -alias serverKey -keyalg RSA -keysize 1024  -validity 365 -keypass password  -keystore serverKeystore.jks -storepass password

> keytool  -genkey -alias serverKey -keyalg RSA -keysize 1024  -validity 365 -keypass password  -keystore serverTruststore.jks -storepass password

> keytool  -genkey -alias clientKey -keyalg RSA -keysize 1024  -validity 365 -keypass password  -keystore clientKeystore.jks -storepass password

> keytool  -genkey -alias clientKey -keyalg RSA -keysize 1024  -validity 365 -keypass password  -keystore clientTruststore.jks -storepass password

- Import server public cert into client truststore:
> keytool -export -rfc -keystore serverKeystore.jks -alias serverKey -file oasis-local-cxf-server.cer -storepass password

> keytool -v -printcert -file oasis-local-cxf-server.cer

> keytool -import -noprompt -trustcacerts -file oasis-local-cxf-server.cer -alias serverKey -keystore clientTruststore.jks -storepass password

- Import client public cert into server truststore:
> keytool -export -rfc -keystore clientKeystore.jks -alias clientKey -file oasis-local-cxf-client.cer -storepass password

> keytool -v -printcert -file oasis-local-cxf-client.cer
> keytool -import -noprompt -trustcacerts -file oasis-local-cxf-client.cer -alias clientKey -keystore serverTruststore.jks -storepass password

- Create p12 certificate for using it in Postman:
> keytool -importkeystore -srckeystore clientKeystore.jks -destkeystore wiremock.p12 -srcstoretype JKS -deststoretype PKCS12 -deststorepass password

- Run Wiremock, we are using docker:
> docker run -v /local/path/to/certificates/files/serverKeystore.jks:/home/wiremock/serverKeystore.jks -v /local/path/to/certificates/files/serverTruststore.jks:/home/wiremock/serverTruststore.jks -it --rm -p 8089:8089 -p 8181:8181 --name wiremock  wiremock/wiremock:2.33.2  --port 8089 --https-port 8181 --verbose --https-keystore ./serverKeystore.jks --keystore-password password --keystore-type jks --https-truststore ./serverTruststore.jks --truststore-type jks --truststore-password password --key-manager-password password --https-require-client-cert

- Add a response in Wiremock:
{% highlight java %}curl --location --request POST 'http://localhost:8089/__admin/mappings' \
--header 'Content-Type: application/json' \
--data-raw '{ "request": 
    {
    "method":"ANY",
    "scheme": "https",
    "url": "/secure"
    }, 
"response": {
    "status":200,
    "body": "Certificate is working!"
    } 
}'{% endhighlight %}

- Add certificate 'wiremock.p12' to Postman
![](https://i.imgur.com/EOJEXsQs.png)

- Disable SSL certification verification
![](https://i.imgur.com/EOJEXsQ.png)

- Now call to https://localhost:8181/secure and you should get 200 and Certificate is working!
![](https://i.imgur.com/ztFR3Drs.png)
