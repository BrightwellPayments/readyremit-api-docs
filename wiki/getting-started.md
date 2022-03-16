---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit v1, a cloud service that performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 loading="lazy"></div>

# About this version

ReadyRemit v1 is a new service, suitable for early adopters. The `v1` is called the *version number*. It corresponds to the `v1` in REST API endpoints like this:

```
/readyremit/v1/recipients
```

In general, a version number means that, although the API (while retaining the version number) may evolve to support new endpoints, query parameters, status codes, etc., the API will remain backward compatible. 

However, during the ReadyRemit early-adoption phase (Spring 2022), in response to suggestions from early adopters, `v1` may experience improvements that are not backward compatible.

All changes will be described in the [Changelog](https://developer.readyremit.com/changelog).

# Signing up for the service

To enable your fintech applications to leverage ReadyRemit, contact Brightwell via the *Get in touch* form on the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page. Fill in your name and email, mention *ReadyRemit* in the comment, and click Submit. Once you sign up, Brightwell will provide you with the following:

* A *client_id* and a *client_secret* for each application that accesses ReadyRemit (required for authentication). 
* A *senderId* for each sender entity you register with ReadyRemit (required by certain API operations).

# Accessing the service

The ReadyRemit service is accessible via a REST API which enables client applications to (1) build relevant forms for end-user input, (2) instantiate receiver and receiver-account objects in ReadyRemit, and (3) make cross-border payments from senders to recipients:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-use-ops.png" width=600 loading="lazy"></div>

The ReadyRemit service supports recipient and sender records. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient records, it does not yet include the same operations for sender records:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

So, early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by some REST API operations.

# OpenAPI and Postman

ReadyRemit provides an OpenAPI file and a Postman Collection file for REST API documentation and experimentation:

* <a href="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-openapi-v1-20220315.yaml" target="_blank">readyremit-openapi-v1-20220315.yaml</a>. This file serves as the source for the [API Reference](https://developer.readyremit.com/reference) on this portal. You can also import this file into your own [SwaggerHub](https://app.swaggerhub.com) account.

* <a href="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-postman-v1-20220315.json" target="_blank">readyremit-postman-v1-20220315.json</a>. You can import this collection file into your [Postman](https://www.postman.com) account.

**These files will change frequently during the early-adoption phase.**

# Primary workflow

The primary workflow focuses on creating a new recipient record and recipient-account record (in ReadyRemit), and sending funds from a previously defined sender account to the new recipient account. Other workflows (not described here) are important, too, like selecting an existing recipient and recipient account, creating a new account for an existing recipient, modifying a recipient record, etc.

## Get an access token

ReadyRemit uses the <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow" target="_blank">Client Credentials Flow</a> of the <a href="https://auth0.com/" target="_blank">Auth0</a> Platform for authentication:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth0.png" width=700 loading="lazy"></div>

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the Auth0 ReadyRemit account, and provides you with a *client_id* and a *client_secret* which, at start up, your app reads from an enviroment file, and trades (via an API call to Auth0) for an *access_token* that, subsequently, gives your app access to ReadyRemit REST API operations:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 loading="lazy"></div>

This *access_token* gives your application access to the ReadyRemit service. It is independent of any authentication workflow your app performs for your end users. To get an access token, call [Get Access Token](https://readyremit.readme.io/reference/getaccesstoken). 

## Get a list of countries

ReadyRemit enables you to populate your user interface with the names and ISO 3-letter codes of countries and (where necessary) currencies:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-countries-and-currencies.png" width=450 loading="lazy"></div>

An [ISO 3-letter country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) and an [ISO 3-letter currency code](https://en.wikipedia.org/wiki/ISO_4217) are factors in determining amounts, fees, and disclosures. To obtain countries and currencies, call [Get Countries](https://readyremit.readme.io/reference/getcountries).

## Get a quote

Establishing a quote is based on the following values:

|Key|Description|
|-|-|
|`dstCountryIso3Code`|Same as Recipient Country obtained in the previous step.|
|`dstCurrencyIso3Code`|Same as Recipient Currency obtained in the previous step.|
|`srcCurrencyIso3Code`|See [ReadyRemit v1: Sender operations](https://readyremit.readme.io/docs/change-log#readyremit-v1-sender-operations).|
|`transferMethod`|You need to obtain this. See [ReadyRemit v1: Transfer methods](https://readyremit.readme.io/docs/change-log#readyremit-v1-transfer-methods).|
|`quoteBy`|Not implemented yet. See [ReadyRemit v1: Quote by sender](https://readyremit.readme.io/docs/change-log#readyremit-v1-quote-by-sender).|
|`amount`|You need to obtain this.|

Many applications obtain sender amount and transfer method from the end user:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-sender-amount-and-transfer-method.png" width=450 loading="lazy"></div>

Once you've obtained the information in the table, call [Get Quote](https://readyremit.readme.io/reference/getquote) to obtain the recipient amount, fees, and disclosures. Then, you can display the quote to the end user for approval:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-quote-form.png" width=470 loading="lazy"></div>

## Get recipient fields

The fields necessary for building a user-facing form to collect recipient information and create recipient records in ReadyRemit vary depending on country, currency, transfer method, and recipient type. See [ReadyRemit v1: Field Types](https://readyremit.readme.io/docs/change-log#readyremit-v1-field-types) for details. First, obtain a [recipient type](https://readyremit.readme.io/docs/change-log#readyremit-v1-recipient-types) from the end user:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient-type.png" width=220 loading="lazy"></div>

Then, to determine required recipient fields, call [Get Recipient Fields](https://readyremit.readme.io/reference/getrecipientfields).

## Create a recipient

Once you obtain the required and optional recipient fields for the previously specified remittance corridor, you can iterate over the response body to build and display a form for the end user:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient.png" width=470 loading="lazy"></div>

Then, call [Create Recipient](https://readyremit.readme.io/reference/createrecipient) to create a recipient record in ReadyRemit.

## Get recipient account fields

The fields necessary for building a user-facing form to collect recipient *account* information also vary depending on the characteristics of the remittance corridor. To discovery the relevant fields, call [Get Recipient Account Fields](https://readyremit.readme.io/reference/getrecipientaccountfields).

## Get banks and branches

The recipient-account form you build for the end user may need to include dropdowns for the banks and associated branches in the specified country. Call [Get Banks](https://readyremit.readme.io/reference/getbanks) and [Get Bank Branches](https://readyremit.readme.io/reference/getbankbranches) to retrieve these arrays. 

## Create a recipient account

Then, with bank and branch information in hand, you can build and present a form:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient-account.png" width=470 loading="lazy"></div>

Then, call [Create Recipient Account](https://readyremit.readme.io/reference/createrecipientaccount) to create a recipient-account record in ReadyRemit.

## Execute the transfer

You should now have all the information you need to execute the transfer:

```
PUT INTO A TABLE
dstCountryIso3Code
dstCurrencyIso3Code
srcCurrencyIso3Code
transferMethod
quoteBy
amount
senderId
recipientId
recipientAccountId
purposeOfRemittance
```

Call [Execute Transfer](https://readyremit.readme.io/reference/executetransfer)