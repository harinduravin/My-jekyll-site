---
layout: post
type: tech
image: https://cdn-images-1.medium.com/max/800/1*3h0nvkUMRGYk9QEjJGpwsQ.jpeg
comments: true
title: An analysis of UK and Australia DCR specifications
description: What is Dynamic Client Registration (DCR)?
date: '2020-11-17T14:57:14.729Z'
categories: []
keywords: []
slug: /@harinduravin/an-analysis-of-uk-and-australia-dcr-specifications-df7348275b4c
---

![](https://cdn-images-1.medium.com/max/800/1*3h0nvkUMRGYk9QEjJGpwsQ.jpeg)

### What is Dynamic Client Registration (DCR)?

In Open banking, Dynamic Client registration takes care about how an Accredited Data Recipient (ADR) can automatically register themselves with Data Holders(DH). Data Holder is a bank that holds large amounts of consumer data. An ADR is a fintech company that is expecting to serve the end consumer with a software or a mobile application. Once Registered, ADRs receive client credentials. These client credentials can be used in the authorization process, where end customer gives consent ( or the permission) to the Data Holder. With the consent, Data Holder can send different consumer related data to the ADR.

To understand DCR better, it is better to dive into detailed specifications given by regulatory authorities of different countries. UK and Australia are well known for implementing open banking.

### Australia:

In Australia, regulatory authorities have come up with a set of standards for implementing this Open banking ecosystem. Background story and a high level description can be found here.

[**Why current retail banking industry should cease for a moment and change?**  
_In Australia, Australian Competition & Consumer Commission (ACCC) is overlooking a new type of regulation for banks in…_medium.com](https://medium.com/@harindu.ravin/why-current-retail-banking-industry-should-cease-for-a-moment-and-change-929ab99fcf09 "https://medium.com/@harindu.ravin/why-current-retail-banking-industry-should-cease-for-a-moment-and-change-929ab99fcf09")[](https://medium.com/@harindu.ravin/why-current-retail-banking-industry-should-cease-for-a-moment-and-change-929ab99fcf09)

Given below describes how the regulatory authorities expect the whole ecosystem to be connected through APIs. Information should flow between three parties.

1.  Data Holders (DH)
2.  Accredited Data Recipients (ADR)
3.  Consumer Data Right Register (This will be ACCC itself)

Since data security is very important in the flow of data between these parties, ACCC has standardized the way data in API calls and responses should be secured.

In _Figure 1_, small circles represent APIs that should be published by the three parties. Half circles represent the entity that calls the APIs to collect data. Each of these API calls should work hand in hand for the Open banking ecosystem to work. This is a very simple diagram. In actual situation, there are many Data Holders as well as Accredited Data Recipients. Only CDR Register(ACCC) will be a unique organization in the ecosystem. CDR Register is responsible for making Data Recipients Accredited.

![](https://cdn-images-1.medium.com/max/800/1*pjphtdAtQNQhHwGebPAWVA.png)

### DCR API

If you look carefully at the figure 1, “Registration APIs” is visible on the Data Holder. Registration Manager of ADR is invoking this API. There are four endpoints available to invoke this API.

![](https://cdn-images-1.medium.com/max/800/1*cjHhCLyB0C6NGur3cheoMg.png)

APIs are invoked through HTTP requests. In the TLS-MA column, all the boxes are ticked. TLS-MA is Transport Layer Security-Mutual Authentication. What this means is that both Data Holder and Accredited Data Recipient should exchange certificates with each other to create a TLS connection.

After the POST request, when the client is registered, a client ID is sent back from the Data Holder. This Client ID is sent back by ADR to request an _access token_ and a _refresh token_.

This access token is used by GET, PUT and DELETE requests along with the Client ID in the end points. Client ID acts as the value for Client Credentials. By looking at the table,

> Client credentials grant type should be used when generating access tokens for the Dynamic Client Registration API.

In the HoK column, last three requests are ticked. HoK is an abbreviation for Holder-of-Key Validation. It is a validation to verify that the scope parameter in the access token contains the fingerprint of the TLS certificate sent in the request. For this, the access token needs to be bound with the MTLS certificate.

In the Access Token Scope column, last three requests are given “cdr:registration”. First request is given none, because an access token is not used in the POST request.

Auth Server Support column

Above security profile is built upon the foundations of the _Financial-grade API Read Write Profile (FAPI-RW)_ and other standards relating to _Open ID Connect 1.0 (OIDC)._

Figure 2 is a detailed illustration of the flows that happen between three parties, in Dynamic Client Registration. POST request described above is highlighted in the flow.

### Before Registration…

Before DCR takes place, ADR must know the API endpoint to call. For example:

> https://digital-api.westpac.com.au/open-banking/0.1/register

To build the endpoint, ADR needs some information about the specific Data Holder. In the example the Data Holder is a large bank in Australia. Details of the bank can be found in the below link.

[**Company overview**  
_A brief overview of the Westpac Group including our history, businesses, vision and values and partners._www.westpac.com.au](https://www.westpac.com.au/about-westpac/westpac-group/company-overview/ "https://www.westpac.com.au/about-westpac/westpac-group/company-overview/")[](https://www.westpac.com.au/about-westpac/westpac-group/company-overview/)

“/open-banking/0.1/register” part is already available. What the ADR needs to collect is “digital-api.westpac.com.au”. To do this, ADR calls a different API in the CDR Register (Figure 1): Discovery APIs.

GetDataHolderBrands — Discovery of DH Brands and their associated endpoints

Now ADR can invoke the DCR API on the Data Holder.

### Registration Flows

![](https://cdn-images-1.medium.com/max/800/1*0V5URo7ngslGt5w6y-9blw.png)

Here is a step by step explanation of the flow,

*   According to ACCC, Software Product Configuration is a manual process performed via the CDR Participant Portal. From this point forward everything is automated.

![](https://cdn-images-1.medium.com/max/600/1*CkNEP4Zllw7dslNPNW8qYg.png)
![](https://cdn-images-1.medium.com/max/600/1*eLAqw9XkK2_U52shZQG-sA.png)

*   In the CDR register SSA(Software Statement Assertion) API (Refer Figure1) is invoked by the ADR. Since the above manual process is already completed, API instantly replies with the SSA belonging to the software. Figure 4 shows an example body of the SSA received. SSA is received as a private key signed JWT.
*   Openid-configuration is requested from the Data Holder by the ADR. Figure 5 shows the request and response. This belongs to InfoSec APIs as shown in the Figure 1. This is called OpenId-connect discovery. It is required for the purpose of discovering an end user’s OpenID provider, and also to obtain information required to interact with the OpenID provider, including its OAuth 2.0 endpoint locations.
*   Now the DCR POST /register request is sent by the ADR to the Data Holder.

> What is JWKS?

> JSON Web Key (JWK):

> A JSON object that represents a cryptographic key. The members of the object represent properties of the key, including its value.

> JWK Set(JWKS):

> A JSON object that represents a set of JWKs. The JSON object MUST have a “keys” member, which is an array of JWKs.

![](https://cdn-images-1.medium.com/max/800/1*x8U5prXSgwCxr77t5hxbGA.png)

*   Data Holder calls JWKS from both CDR Register and Accredited Data Recipient. For this InfoSec APIs are invoked.
*   Data holder calls CDR-Register JWKS endpoint which is secured using TLS to retrieve the _public key_ to validate the SSA signature. Relavent key is identified by matching “kid” in the SSA and the JWKS.
*   Similarly, Data Holder searches through JWKS retrieved from ADR to validate the POST /register request JWT as well.
*   After validation, registration JWT claims are checked.
*   Finally registration takes place in the Identity Provider. Data Holder replies the ADR with the client ID, which will be used as client credential grant type for retrieving access token.

### UK:

Australia Open Banking standards are based under UK standards. Competition and Markets Authority (CMA) set the initiative to implement Open Banking in order to allow smaller banks better compete with large banks. Open Banking implementation is controlled by OBIE (Open Banking Implementation Entity) in the UK. After Open Banking standards were issued, version 2 of the Payment Services Directive(PSD2) was also issued.

Most of the DCR specifications are very similar to Australia. Some terms are referred differently in the two countries. Given below are some important acronyms and terms.

1.  Account Servicing Payment Service Providers(ASPSP )= Data Holder(DH)
2.  Third Party Provider (TPP) = Accredited Data Recipient (ADR) (AISP, PISP, PIISP)
3.  OBIE Directory acts as the ACCC’s CDR Register
4.  AISP = Account Information Service Provider (Account Information)
5.  PISP = Payment Initiation Service Provider (Payment Initiation)
6.  PIISP = Payment Instrument Issuer Service Provider
7.  CBPII = Card Based Payment Instrument Issuer (UK- Confirmation of funds)
8.  PSP = Payment Service Provider(AISP, PISP, CBPII, ASPSP)

Different Standards used,

1.  GDPR — Global Data Protection Regulation( Replaced previously used EU Data Protection Directive)
2.  RTS — Regulatory Technical Standards(PSD2 refers to RTS for technical guidance)
3.  FAPI — Financial-grade API(Oauth2.0 + OIDC + additional technical requirements)

In the UK specifications allow two methods for registration.

1.  Dynamic Client Registration
2.  Manual Client Registration

![](https://cdn-images-1.medium.com/max/800/0*pwjSxeLl5efXs3SW.png)

According to the specification, PTC is the person at the TPP who creates an SSA and invokes a registration mechanism. What we are referring in the article is 1>2>3A>4A>5A.

### Minor differences between AU and UK DCR API requests

Both Software Statement Assertion and DCR POST request are the most significant in understanding DCR. Specification level differences and similarities of them can be identified using the references given below. Following are the differences identified.

![](https://cdn-images-1.medium.com/max/800/1*HZLNthJ7Bd7jNZuWp427LA.png)
![](https://cdn-images-1.medium.com/max/800/1*WU1lgLCuzFKZDJYXgxj0sw.png)

None of the organization claims are available AU specification.

![](https://cdn-images-1.medium.com/max/800/0*GvbnCHJYI4_9BCjB)

Some of the claims are similar to AU specification. There are claims about the software that is not available in AU specification.

![](https://cdn-images-1.medium.com/max/800/1*JgaEwuVFqTMODxxu13x4vg.png)

### References

[**API Security for AU**  
_Skip to end of metadata Go to start of metadata The information security profile is built upon the foundations of the…_docs.wso2.com](https://docs.wso2.com/display/OB150/API+Security+for+AU "https://docs.wso2.com/display/OB150/API+Security+for+AU")[](https://docs.wso2.com/display/OB150/API+Security+for+AU)

[**CDR Register Design Reference**  
_The Australian Government is introducing a Consumer Data Right (CDR) to give consumers more control over their data…_cdr-register.github.io](https://cdr-register.github.io/register/#registration-response "https://cdr-register.github.io/register/#registration-response")[](https://cdr-register.github.io/register/#registration-response)

[**OpenID Connect Discovery**  
_WSO2 Identity Server supports OpenID Connect Discovery to discover an end user's OpenID provider, and also to obtain…_docs.wso2.com](https://docs.wso2.com/display/IS570/OpenID+Connect+Discovery "https://docs.wso2.com/display/IS570/OpenID+Connect+Discovery")[](https://docs.wso2.com/display/IS570/OpenID+Connect+Discovery)

[**Confluence**  
_Edit description_openbanking.atlassian.net](https://openbanking.atlassian.net/wiki/spaces/DZ/pages/36667724/OpenBanking+OpenID+Dynamic+Client+Registration+Specification+-+v1.0.0-rc2 "https://openbanking.atlassian.net/wiki/spaces/DZ/pages/36667724/OpenBanking+OpenID+Dynamic+Client+Registration+Specification+-+v1.0.0-rc2")[](https://openbanking.atlassian.net/wiki/spaces/DZ/pages/36667724/OpenBanking+OpenID+Dynamic+Client+Registration+Specification+-+v1.0.0-rc2)