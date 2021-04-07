---
layout: post
type: tech
image: https://cdn-images-1.medium.com/max/800/1*_DkcB2a_15K39gpPs0VAKg.png
comments: true
title: How to decrypt a JSON Web Encryption (JWE) using JAVA
description: >-
  JSON Web Token (JWT) defines a container to transport data between interested parties. It became an IETF standard in May 2015 with the RFC 7519. There are multiple applications of JWT. The OpenID Connect is one of them. In OpenID Connect the id token is represented as a JWT. Both in securing APIs and Microservices, the JWT is used as a way to propagate and verify end-user identity.
date: '2020-12-11T13:56:58.050Z'
categories: []
keywords: []
slug: >-
  /@harinduravin/how-to-decrypt-a-json-web-encryption-jwe-using-java-4146ef3082a6
---

![](https://cdn-images-1.medium.com/max/800/1*_DkcB2a_15K39gpPs0VAKg.png)

> JSON Web Token (JWT) defines a container to transport data between interested parties. It became an IETF standard in May 2015 with the RFC 7519. There are multiple applications of JWT. The OpenID Connect is one of them. In OpenID Connect the _id\_token_ is represented as a JWT. Both in securing APIs and Microservices, the JWT is used as a way to propagate and verify end-user identity.

Above description is a direct reference from,

{% linkpreview "https://medium.facilelogin.com/jwt-jws-and-jwe-for-not-so-dummies-b63310d201a3" %}

There are two types of encryption,

1.  Symmetric encryption
2.  Asymmetric encryption

![](https://cdn-images-1.medium.com/max/800/1*qFcIscbuAMf80JHFkep97Q.png)

A quick youtube video or google search can help one understand this concept. These two types of encryption exist in JSON Web Tokens (JWT) as well. As explained above, JWTs are used as containers to transport data between two parties.

Let’s say for example your mobile banking app is acquiring banking data from the bank to your phone. Some data might be very confidential. You do not want anyone to get their hands on them while in the network.

For this purpose, **JWEs** can be used. To communicate using JWEs, **both symmetric and asymmetric** encryption is used.

Some data might be not so critical as well, but the banks may need to know for sure that the data you send is actually from you.

For this purpose, **JWS** (JSON Web Signature) can be used. If anyone in the internet get their hands on with this data, they can know exactly what you sent, because you have encoded the data (but not encrypted) with a famous encoding method(base64url).

![](https://cdn-images-1.medium.com/max/800/1*xQ-xuOiV8O9doAcSiqKPOQ.png)

JWS can be generated easily using the following website,

{% linkpreview "https://jwt.io/" %}

To create a JWS, data is represented as JSON and then signed using the sender’s private key. The receiver can then use the sender’s public key (which is available without restriction) and validate the sender.

Using JWT.io site to create JWS and validate JWS is the easy part. Sometimes we need to send data using JWEs. Here, actual encryption takes place in addition to usual base64url encoding.

JWE is more complex than this. It has 5 parts connected using periods.

![](https://cdn-images-1.medium.com/max/800/1*lgMjCBtkaH1PiH8jy4zpyg.png)

Say you receive a JWE, encrypted using your public key. Now since you have your private key, only you should be able to decrypt with your private key. you can decrypt the second part (Encrypted Key) in the image. This can be confusing. The second section in the image is the encrypted encryption key of the payload (data).

In order to obtain data, we need the **key** ( Symmetric encryption key) which is encrypted using an asymmetric encryption key. As explained earlier, this is the use of both symmetric and asymmetric keys.

Unlike JWS, there seems to be no reputable sites for the purpose of encrypting and decrypting JWEs. This is due to obvious security risk of getting private keys public.

\-----BEGIN PRIVATE KEY-----  
MIIEowIBAAKCAQEAvpnaPKLIKdvx98KW68lz8pGaRRcYersNGqPjpifMVjjE8LuC  
oXgPU0HePnNTUjpShBnynKCvrtWhN+haKbSp+QWXSxiTrW99HBfAl1MDQyWcukoE  
b9Cw6INctVUN4iRvkn9T8E6q174RbcnwA/7yTc7p1NCvw+6B/aAN9l1G2pQXgRdY  
C/+G6o1IZEHtWhqzE97nY5QKNuUVD0V09dc5CDYBaKjqetwwv6DFk/GRdOSEd/6b  
W+20z0qSHpa3YNW6qSp+x5pyYmDrzRIR03os6DauZkChSRyc/Whvurx6o85D6qpz  
ywo8xwNaLZHxTQPgcIA5su9ZIytv9LH2E+lSwwIDAQABAoIBAFml8cD9a5pMqlW3  
f9btTQz1sRL4Fvp7CmHSXhvjsjeHwhHckEe0ObkWTRsgkTsm1XLu5W8IITnhn0+1  
iNr+78eB+rRGngdAXh8diOdkEy+8/Cee8tFI3jyutKdRlxMbwiKsouVviumoq3fx  
OGQYwQ0Z2l/PvCwy/Y82ffq3ysC5gAJsbBYsCrg14bQo44ulrELe4SDWs5HCjKYb  
EI2b8cOMucqZSOtxg9niLN/je2bo/I2HGSawibgcOdBms8k6TvsSrZMr3kJ5O6J+  
77LGwKH37brVgbVYvbq6nWPL0xLG7dUv+7LWEo5qQaPy6aXb/zbckqLqu6/EjOVe  
ydG5JQECgYEA9kKfTZD/WEVAreA0dzfeJRu8vlnwoagL7cJaoDxqXos4mcr5mPDT  
kbWgFkLFFH/AyUnPBlK6BcJp1XK67B13ETUa3i9Q5t1WuZEobiKKBLFm9DDQJt43  
uKZWJxBKFGSvFrYPtGZst719mZVcPct2CzPjEgN3Hlpt6fyw3eOrnoECgYEAxiOu  
jwXCOmuGaB7+OW2tR0PGEzbvVlEGdkAJ6TC/HoKM1A8r2u4hLTEJJCrLLTfw++4I  
ddHE2dLeR4Q7O58SfLphwgPmLDezN7WRLGr7Vyfuv7VmaHjGuC3Gv9agnhWDlA2Q  
gBG9/R9oVfL0Dc7CgJgLeUtItCYC31bGT3yhV0MCgYEA4k3DG4L+RN4PXDpHvK9I  
pA1jXAJHEifeHnaW1d3vWkbSkvJmgVf+9U5VeV+OwRHN1qzPZV4suRI6M/8lK8rA  
Gr4UnM4aqK4K/qkY4G05LKrik9Ev2CgqSLQDRA7CJQ+Jn3Nb50qg6hFnFPafN+J7  
7juWln08wFYV4Atpdd+9XQECgYBxizkZFL+9IqkfOcONvWAzGo+Dq1N0L3J4iTIk  
w56CKWXyj88d4qB4eUU3yJ4uB4S9miaW/eLEwKZIbWpUPFAn0db7i6h3ZmP5ZL8Q  
qS3nQCb9DULmU2/tU641eRUKAmIoka1g9sndKAZuWo+o6fdkIb1RgObk9XNn8R4r  
psv+aQKBgB+CIcExR30vycv5bnZN9EFlIXNKaeMJUrYCXcRQNvrnUIUBvAO8+jAe  
CdLygS5RtgOLZib0IVErqWsP3EI1ACGuLts0vQ9GFLQGaN1SaMS40C9kvns1mlDu  
LhIhYpJ8UsCVt5snWo2N+M+6ANh5tpWdQnEK6zILh4tRbuzaiHgb  
\-----END PRIVATE KEY-----

This is not a real private key, but private keys look like this.

Let’s say we receive a JWE. By looking carefully, five parts of the JWE separated by four dots can be observed.

> “eyJraWQiOiJEd01LZFdNbWo3UFdpbnZvcWZReVhWenlaNlEiLCJlbmMiOiJBMTI4R0NNIiwiYWxnIjoiUlNBMV81In0.H7oZNGtNPSPZWSaZWZjCHICIfc06w7h0XeJwVWmbJDFqtzIYCXSxyj-DYrVfP9lEmgWglv-WvYMnYEKxbnmrCu1mzGc-QjK4eHEMN\_Z3sD4O-xZjGNVeMCCqxSS6pSTCoRUXTkpl2ENllfEtrnXzqj2xQQMfi98UPTRf3EXKjKt3Ks3x44KQLxk1cYvFuxDzzC0p5Kt1HEeinuzuQHTuPc0uoRC0RhUrGsRTgWkQDBqEDUDFUIE4GY9wQ1sJ1f9ulvtH4NHcHJVvM97aUmHL04bCzoulbWUKzmFxXOBSo41psegzRUWYPVg\_m9dXHvJM\_txY8gCypiU3AXhXT0mVfg.fdfDTkGkjGCyxuYs.gFT6LnqcVw5dV2gaOl0NylpRv89ThLcB3EQ2wFubuFDGuc9BFx16KNlObXkAcqAM\_oZtijErN48IPfZFtCoXrSFDzOiLK1zeHegeW8G8sQkR45XXGvqf\_dvR7cm5R9MzIPQhYZ4VilKbtlOwOlmQHz2FfxKTpEqXzMhjBIwW767wy4VR-mKIn1\_8Nv2pGEb6g7EBmGbBtOb6o9kq4aip0nrh\_KYNF9KLsJd1Ysma7gs-wAvX4siZXQbNMNlGe4DV14j3atW4ChsSdKYXGCze4W5SwqQemGMsHvLWv4wFWObxW0Xbo2nizecemibp-LIlQZ5QM-66xMb4tgysUDRZhoOWhJBPBmMSKyslwOlgpbzn20SYzaqNxRW9wIHG4kSu3cpeEbBh-X1HEjf8ULCBz5Jxac-tM46btxM\_zxDaEFqpgM\_7oKWC5Kv0TQn3V4OUk8RHhAdEF9pmsPKxyx\_tvSgXpj2l7BwDCIxdEwzWSMJYI0ed-p8D5w5ZRYb8DS4.kCMw9Xn9eGEitd8MQio0bA”

Our goal is to decrypt the above JWE using a private key like the previous picture. Below code can be used to decrypt and retrieve the data from the JWE. Make sure to start the project as a Maven project in order to add dependencies when required.

![](https://cdn-images-1.medium.com/max/800/1*osCD8dQ3ueH2DnTGDhxsUw.png)

_Access the JAVA project through this link:_

[_https://github.com/harinduravin/JWEdecrypt_](https://github.com/harinduravin/JWEdecrypt)

Modulus and private exponent parts are a result of converting the Private key string to a friendly format (RSA format). Converting the private key was essential for the actual decryption part. Actual decryption process is complex and involve mathematics. Different algorithms used in these can be studied as well.

The second part of the response is the decrypted payload, which is an id token. As explained in the first paragraph, OpenID Connect is one of the applications of JWT. An id token is a part of OpenID Connect. The payload of the response shows the claims expected from this id token.

If the private key is not matching with the public key used by sender, an error will occur in the following code line.

**jwt.decrypt(decrypter);**

References:

{% linkpreview "https://is.docs.wso2.com/en/5.9.0/learn/decrypting-openid-connect-encrypted-id-tokens/" %}