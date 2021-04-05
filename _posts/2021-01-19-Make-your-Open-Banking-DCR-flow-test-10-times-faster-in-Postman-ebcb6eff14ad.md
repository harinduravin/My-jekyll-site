---
layout: post
type: tech
image: https://cdn-images-1.medium.com/max/600/0*LQBZ1s7jkWdae94F.png
comments: true
title: Make your Open Banking DCR flow test 10 times faster in Postman
description: >-
  I’m assuming you are already familiar with “What is DCR in open banking?”
  before start reading this article. In case you are not familiar…
date: '2021-01-19T09:01:26.639Z'
categories: []
keywords: []
slug: >-
  /@harinduravin/make-your-open-banking-dcr-flow-test-10-times-faster-in-postman-ebcb6eff14ad
---

I’m assuming you are already familiar with “What is DCR in open banking?” before start reading this article. In case you are not familiar, please use the link here for one of my previous articles on [DCR (Dynamic Client Registration)](https://harinduravin.medium.com/an-analysis-of-uk-and-australia-dcr-specifications-df7348275b4c).

![](https://cdn-images-1.medium.com/max/600/0*LQBZ1s7jkWdae94F.png)

#### What is postman?

Postman is a popular API client that makes it easy for developers to create, share, test and document APIs. This is done by allowing users to create and save simple and complex HTTP/s requests, as well as read their responses. The result — more efficient and less tedious work.

DCR flow consist of these steps.

1.  Sending a POST request containing a JWT body that includes an SSA (Software Statement Assertion)
2.  Using the client ID received from the POST request to send another POST request to get an access token.
3.  Using the access token to send PUT, GET and DELETE requests.

This flow is tedious due to following reasons.

POST and PUT requests are JWTs signed using private keys. This signing process usually require the person to use jwt.io website and manually sign them in the web page. This involves a lot of copying and pasting.

Even the POST request that request for access token need to include a JWT called **client assertion.** This is a onetime use JWT and this needs to be changed and signed all the time.

There is additional copying and pasting as well.

When errors occur due to various reasons, the whole process can get very hard to cope up with. Troubleshooting becomes very tedious.

The solution is to use **Pre-request Scripts😃** as facilitated by Postman software.

Let’s start the Postman software and get started with POST request.

For the demonstration here, I’m using **UK DCR** flow. If you need to see the power of Pre-request Scripts, try to invoke Postman API calls without using them. Additionally, going through the steps given in the below article as explained by WSO2 Open Banking documentation will help you understand the process much better.

[**Dynamic Client Registration v3.2**  
_Before you begin: Deploy the Dynamic Client Registration (DCR) API v3.2. Click here to see how to deploy the DCR API To…_docs.wso2.com](https://docs.wso2.com/display/OB200/Dynamic+Client+Registration+v3.2 "https://docs.wso2.com/display/OB200/Dynamic+Client+Registration+v3.2")[](https://docs.wso2.com/display/OB200/Dynamic+Client+Registration+v3.2)

POST request

In the Postman software create a new request and fill the URL part and Headers tab as below. Since you are sending a POST request, set the request type to POST.

![](https://cdn-images-1.medium.com/max/800/1*NQ0EtQHoQAwabq3Lgi91hQ.png)

Now the most important part in making the flow x10 faster is learning to use environments and variables in Postman.

#### Using environments and variables

![](https://cdn-images-1.medium.com/max/800/1*lHezStrtYb2Eo0gvqMik8w.png)

Above is a screenshot of the upper right corner of the Postman software. Please add the following variable names as given below. After creating the environment UK\_DCR\_3.3, add global variables in addition to environment variables.

![](https://cdn-images-1.medium.com/max/800/1*dfKmwe3wRYDjEQlM5GltdA.png)

Use of variables and environments make the process very fast by removing the need to copy and paste so many values. Setting the values here and referring them by their variable names is very convenient.

We need to worry about setting values for software\_statement, private\_key and jsrasign-js. Others are automatically set by the Pre-request Scripts that we are going to use later.

Copy and paste the following text in the software\_statement field.

{% gist 0d1c04ea53898e9b452226013a03252b %}

Copy and paste the following text in the private\_key field.

{% gist aba4a178ec5c1dd2c0b29980e6388dd7 %}

Copy and paste the following JavaScript code in the jsrasign-js field. This is a JavaScript that runs and creates JWTs out of JSON texts using the private key given.

{% gist f89e2a404fd52eb3a4151e2994eb22d9 %}

With these values in place, now you are ready to prepare the rest of the POST request. Write {% raw %}{{post\_body}} to call this variable in the Body tab.{% endraw %}

![](https://cdn-images-1.medium.com/max/800/1*Ek6kmsUsUX1mmCH0yDmROg.png)

Pre-request Script tab is a little bit tricky. This is a JavaScript code that runs before the request is sent.

![](https://cdn-images-1.medium.com/max/800/1*lxNew_vb9UtqiH65wdrBug.png)

Add the following code in the Pre-request Script tab.

{% gist ed1c61675c5f42a66b2f5b5466266fa3 %}

Now your POST request is ready to run. Let’s add this additional JavaScript code in the Test tab. This will provide functionality to grab the client ID received in the response, when the response is received.

![](https://cdn-images-1.medium.com/max/800/1*1n2aCEzmXGYKXawA8-uXyg.png)

{% gist e3598a41000a22ee8e97620d08d8c600 %}

If not for the first attempt, you will be able to get this POST request working after some troubleshooting. Just remember that this seems complex at first glance, but using JavaScript will make the process a lot faster.

Let’s move on to the next request in the DCR flow, which is the request for obtaining the access token.

#### Obtaining the access token

Try to create a new request as given in the following screenshots.

![](https://cdn-images-1.medium.com/max/800/1*chDOIdxwji7BEFZM_37dUQ.png)
![](https://cdn-images-1.medium.com/max/600/1*sPwMJA0UydaG8IdMjPtGJA.png)


![](https://cdn-images-1.medium.com/max/600/1*84_kkxP8MsXKElUJfEf8Cg.png)
![](https://cdn-images-1.medium.com/max/800/1*STnUjHmp8AnBJ2hQ9v7P8w.png)

Given below are the Pre-request Script and Test script for this request.

{% gist 8bf42b63f6c6c07fa72870606e19467 %}

{% gist 4cd9b91d2305af889a81cb7d65840d69 %}

The Test script grabs the access token received from the response. Now using this access token we can send PUT, DELETE and GET requests. After getting thorough with first two requests I explained above, PUT-GET-DELETE requests will not be a big challenge. You will definitely be able to experiment and get these last three requests done. Given below is a screenshot of the PUT request.

![](https://cdn-images-1.medium.com/max/800/1*wJbcBWdxcM5PCynHjiX93w.png)

Good luck troubleshooting and going through the rest of the DCR UK flow!

#### References

1.  [https://www.blazemeter.com/blog/how-use-postman-manage-and-execute-your-apis](https://www.blazemeter.com/blog/how-use-postman-manage-and-execute-your-apis)
2.  [https://docs.wso2.com/display/OB200/Dynamic+Client+Registration+v3.2](https://docs.wso2.com/display/OB200/Dynamic+Client+Registration+v3.2)