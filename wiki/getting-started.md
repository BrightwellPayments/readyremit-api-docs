---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

This website explains to [fintech](https://en.wikipedia.org/wiki/Financial_technology) application developers how to programmatically leverage, via a REST API, the ReadyRemit Cloud Service which performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors):

<div><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 height=405 loading="lazy"></div>

# Sign up

To enable your fintech applications to leverage ReadyRemit, contact Brightwell via the *Get in touch* form on the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page. Fill in your name and email, mention *ReadyRemit* in the comment, and click Submit. Once you sign up, Brightwell will provide you with the following:

    * A *client_id* and a *client_secret* (required for authentication) for each of your ReadyRemit client applications. 
    * A unique *senderId* (required by certain API operations) for each sender entity you register.
    * Access to the ReadyRemit Postman Collection described in the corresponding Postman Documentation.

# Authentication

ReadyRemit uses the <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow" target="_blank">Client Credentials Flow</a> of the <a href="https://auth0.com/" target="_blank">Auth0</a> Platform for authentication:

<div><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth0.png" width=700 height=346 loading="lazy"></div>

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the Auth0 ReadyRemit account, and provides you with a *client_id* and a *client_secret* which, at start up, your app reads from an enviroment file, and trades (via an API call to Auth0) for an *access_token* that, subsequently, gives your app access to ReadyRemit REST API operations:

<div><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 height=100 loading="lazy"></div>

This *access_token* is independent of any authentication workflow your app performs for end users.

# Recipients and senders

The ReadyRemit service supports recipient and sender entities. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient entities, it does not yet include the same operations for sender entities:

<div><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 height=436 loading="lazy"></div>

So, for the time being, ReadyRemit early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by various REST API operations. 

# Variable fields

The fields necessary for building user-facing forms to collect recipient information (and, subsequently, to `POST` recipient and recipient-account data to ReadyRemit) vary depending on country, currency, transfer method, and recipient type. For example, the fields necessary to define a recipient with a bank account in India (Indian Rupee) differ from those necessary to define a recipient in the Philippines (Philippine Peso) expecting a cash remittance:

<div><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-variable-fields.png" width=600 height=307 loading="lazy"></div>

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

# Sending money

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#231a6946-f65e-4d25-bb45-8192da72177e" target="_blank">Create Access Token</a> to obtain an access token required by all other ReadyRemit REST API operations.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#8f01eb59-3eb5-4eea-8e5a-f7f5e97e4d36" target="_blank">Get Countries and Currencies</a> to get a list of the recipient countries/currencies you support.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#0b582301-8de2-4610-88ba-df0fad04f024" target="_blank">Get Quote</a> to return the recipient amount (based on the sender amount and the exchange rate) and fees.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#4ec3357f-a4a5-4f53-aca8-1d2c7049a636" target="_blank">Get Recipient Fields</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#7ecc57ba-7c37-49ee-b333-b273402d455a" target="_blank">Create Recipient</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#c0c343a1-9a82-48ff-81a5-4a8e5397e503" target="_blank">Get Banks</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#0baa42f9-245f-4d1f-8fe8-b1cf795e851d" target="_blank">Get Bank Branches</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#653ea7bf-e34b-4272-8d49-8d84ba7a7d34" target="_blank">Get Recipient Account Fields</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#b8d89873-5827-456e-a2d7-b94f6de7b078" target="_blank">Create Recipient Account</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#a8d5f9cd-9f84-41c7-8fd4-b9356559c35f" target="_blank">Execute Transfer</a> ...
