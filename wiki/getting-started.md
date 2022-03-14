---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit, a cloud service that performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 loading="lazy"></div>

To enable your fintech applications to leverage ReadyRemit, contact Brightwell via the *Get in touch* form on the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page. Fill in your name and email, mention *ReadyRemit* in the comment, and click Submit. Once you sign up, Brightwell will provide you with the following:

* A *client_id* and a *client_secret* for each application that accesses ReadyRemit (required for authentication). 
* A *senderId* for each sender entity you register with ReadyRemit (required by certain API operations).

Applications use ReadyRemit REST API operations to make cross-border payments from senders to recipients. The operations enable applications to (1) build relevant forms for end-user input, (2) instantiate receiver and receiver-account objects in ReadyRemit, and (3) make payments:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-use-ops.png" width=600 loading="lazy"></div>

## Get an access token

ReadyRemit uses the <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow" target="_blank">Client Credentials Flow</a> of the <a href="https://auth0.com/" target="_blank">Auth0</a> Platform for authentication:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth0.png" width=700 loading="lazy"></div>

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the Auth0 ReadyRemit account, and provides you with a *client_id* and a *client_secret* which, at start up, your app reads from an enviroment file, and trades (via an API call to Auth0) for an *access_token* that, subsequently, gives your app access to ReadyRemit REST API operations:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 loading="lazy"></div>

This *access_token* gives your application access to the ReadyRemit service. It is independent of any authentication workflow your app performs for your end users.

To get an access token, call [Get Access Token](https://readyremit.readme.io/reference/getaccesstoken). 

## Get a list of countries

Call [Get Countries](https://dash.readme.com/project/readyremit/v0.1/refs/getcountries) to obtain an array of countries and relevant currencies. You can display this content to end users for selection.

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-countries-and-currencies.png" width=450 loading="lazy"></div>

## Get a quote

Obtain the send amount and the transfer method from the end user.

Call [Get Quote](https://readyremit.readme.io/reference/getquote) to obtain the recipient amount, fees, and disclosures. You can display the quote to the end user for approval.

## Get recipient fields

Obtain the recipient type from the end user.

Call [Get Recipient Fields](https://readyremit.readme.io/reference/getrecipientfields).

The fields necessary for building user-facing forms to collect recipient information (and, subsequently, to `POST` recipient and recipient-account data to ReadyRemit) vary depending on country, currency, transfer method, and recipient type. For example, the fields necessary to define a recipient with a bank account in India (Indian Rupee) differ from those necessary to define a recipient in the Philippines (Philippine Peso) expecting a cash remittance:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-variable-fields.png" width=600 loading="lazy"></div>

To accommodate, ReadyRemit provides two operations, *Get Recipient Fields* and *Get Recipient Account Fields*, that return response bodies containing sets of field definitions over which client applications iterate to build user-facing forms. Here is an example:

``` json
{
  "fieldSets": [
    {
      "fieldSetId": "PERSONAL_INFORMATION",
      "fieldSetName": "Personal information",
      "fields": [
        {"fieldType": "PHONE_NUMBER", "fieldId": "PHONE_NUMBER", "name": "Phone Number", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "FIRST_NAME", "name": "First Name", "isRequired": true },
        {"fieldType": "TEXT", "fieldId": "EMAIL", "name": "Email", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "NATIONALITY", "name": "Nationality", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "LAST_NAME", "name": "Last Name", "isRequired": true },
        {"fieldType": "TEXT", "fieldId": "DATE_OF_BIRTH", "name": "Date of Birth", "isRequired": false }
      ]
    },
    {
      "fieldSetId": "...",
      "fieldSetName": "...",
      "fields": [
        {"fieldType": "...", "fieldId": "...", "name": "...", "isRequired": false },
        {"fieldType": "...", "fieldId": "...", "name": "...", "isRequired": false }
      ]
    }
  ]
}
```

Supported field types are the following:

* `DATE`
* `DROPDOWN`
* `PHONE_NUMBER`
* `TEXT`

## Create a recipient

Call [Create Recipient](https://readyremit.readme.io/reference/createrecipient).

The ReadyRemit service supports recipient and sender entities. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient entities, it does not yet include the same operations for sender entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

So, for the time being, ReadyRemit early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by various REST API operations (e.g. [Create Recipient](https://readyremit.readme.io/reference/createrecipient)).

## Get recipient account fields

Call [Get Recipient Account Fields](https://readyremit.readme.io/reference/getrecipientaccountfields).

## Get banks and branches

Call [Get Banks](https://readyremit.readme.io/reference/getbanks).

Call [Get Bank Branches]().

## Create a recipient account

Call [Create Recipient Account](https://readyremit.readme.io/reference/createrecipientaccount).

## Execute the transfer

Call [Execute Transfer](https://readyremit.readme.io/reference/executetransfer)