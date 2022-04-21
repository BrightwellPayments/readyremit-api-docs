---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit, a cloud service that performs cross-border [remittances](https://en.wikipedia.org/wiki/Remittance) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=600 loading="lazy"></div>


## Interfaces

ReadyRemit offers a [REST API](/reference). Language-specific SDKs are on the roadmap.

## Sandbox

ReadyRemit is available in a [sandbox](https://sandbox-api.readyremit.com) environment where you can use the API Reference, SwaggerHub, or Postman to experiment with API operations. See the [API Reference](/reference) for details.

You can also use the ReadyRemit service in the sandbox environment as you integrate your service.

## Versioning

The current ReadyRemit version is ReadyRemit v1. The `v1` corresponds to the `v1` in REST API endpoints like this:

```
/v1/recipients
```

In general, a version number like `v1` means that, although the ReadyRemit service may experience several subsequent releases to support new endpoints, query parameters, status codes, etc., the API will remain backward compatible. However, during the ReadyRemit early-adoption phase (Spring 2022), in response to suggestions from early adopters, `v1` may experience improvements that are not backward compatible.

## Releases

Periodically, the ReadyRemit Team pushes a new ReadyRemit release to the [sandbox](https://sandbox-api.readyremit.com) environment and publishes a corresponding [release note](https://developer.readyremit.com/changelog).

## Authentication

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the ReadyRemit account of an authentication service:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-authentication.png" width=700 loading="lazy"></div>

Brightwell also provides you with credentials (i.e. `client_id`, `client_secret`) which your app trades (via [Get Access Token](/reference/getaccesstoken)) for an *access_token* that, subsequently, gives your app access to all the other ReadyRemit REST API operations:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 loading="lazy"></div>

Note that this authentication workflow is independent of any authentication workflow your app performs for your end users.

# Your service is the sender

For this use case, your service is the sender, and your end users are the recipients. The use case supports transfers initiated by your service (e.g. payroll) and transfers initiated by your end users (e.g. reward redemption). Once fully implemented, ReadyRemit will represent your service with a *Sender* entity, and each of your end users with a *Recipient* entity and one or more *Recipient Account* entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-service-is-the-sender-4.png" width=800 loading="lazy"></div>

Note the *senderId* in the diagram above. You obtain a *senderId* for your service when you sign up as a ReadyRemit client. You can store the *senderId* in an environment file (read at startup) or in your database (especially if you need to restrict access to it). 

## Using the senderId

Your service uses the *senderId* to call [Create Recipient](/reference/createrecipient) for each end user, storing the returned *recipientIds* in a *Users* table (as shown in the diagram above) or in a related table:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-pseudocode-alpha-3.png" height=125 loading="lazy"></div>

A *Recipient* entity represents the person to whom the transfer will be sent. 

## Using recipientIds

Recall that your service obtains (via [Create Recipient](/reference/createrecipient)) and stores a *recipientId* for each user in your database:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipientid-to-recipient-map.png" width=800 loading="lazy"></div>

Your service uses each *recipientId* to call (on behalf of the user) [Create Recipient Account](/reference/createrecipientaccount) to create accounts, and [Get Recipient Accounts](/reference/getrecipientaccounts) to retrieve an array of these accounts:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-pseudocode-gamma-2.png" height=149 loading="lazy"></div>

ReadyRemit maintains *Recipient <> Recipient Account* relationships. This means, rather than remembering these relationships itself, your service can call [Get Recipient Accounts](/reference/getrecipientaccounts) to get all the *Recipient Accounts* related to a particular *recipientId*. 

## Using recipientAccountIds

Your service uses *recipientAccountIds* to call [Get Recipient Account](/reference/getrecipientaccount), [Update Recipient Account](/reference/updaterecipientaccount), and [Delete Recipient Account](/reference/deleterecipientaccount):

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-pseudocode-delta.png" height=175 loading="lazy"></div>

## Workflow

1. Call [Get Access Token](/reference/getaccesstoken) to retrieve an access token needed for calls to all other API operations.


1. Call [Get Countries and Currencies](/reference/getcountriesandcurrencies) and [Get Quote](/reference/getquote) to build, display, and execute a quote form so that the user can consider current exchange rates and fees.

1. Call [Get Recipient Fields](/reference/getrecipientfields) which, based on the supplied country code, currency code, etc., returns required and optional fields for creating a *Recipient* entity. If you need your end user to supply values for these fields, you can build and present a form to the user.

1. Call [Create Recipient](/reference/createrecipient) to obtain a *recipientId* for each of your end users, keeping in mind the caveats mentioned in the preceding sections about using identifiers. You can also add various field objects as needed. Here is an example of a valid request body:

    ```
    {
      "dstCountryIso3Code": "IND",
      "dstCurrencyIso3Code": "INR",
      "recipientType": "PERSON",
      "transferMethod": "BANK_ACCOUNT",
      "senderId": senderId,
      "fields": [
        {
          "id": "FIRST_NAME",
          "type": "TEXT",
          "value": "Jose"
        },
        {
          "id": "LAST_NAME",
          "type": "TEXT",
          "value": "Martinez"
        }
      ]
    };
    ```

1. Call [Get Recipient Account Fields](/reference/getrecipientaccountfields) which, based on the supplied country code, currency code, etc., returns required and optional fields for creating a *Recipient Account* entity. If you need your end user to supply values for these fields, you can build and present a form to the user. You may need to call [Get Banks](/reference/getbanks) and [Get Bank Branches](/reference/getbankbranches) to populate certain elements in this form.

1. Call [Create Recipient Account](/reference/createrecipientaccount) to create a *Recipient Account* entity. Remember that the user may want to create multiple accounts. Here is an example request body:

    ```
    {
      "dstCountryIso3Code": countryIso3Code,
      "dstCurrencyIso3Code": currencyIso3Code,
      "transferMethod": "BANK_ACCOUNT",
      "senderId": senderId,
      "name": accountName,
      "accountNumber": accountNumber,
      "fields": [
        {
          "id": "PURPOSE_OF_PAYMENT",
          "type": "TEXT",
          "value": "Gift"
        },
        {
          "id": "BANK_NAME",
          "type": "TEXT",
          "value": "Bank of Indonesia"
        }
      ]
    };
    ```

1. Call [Get Recipient Accounts](/reference/getrecipientaccounts) to get an array of *Recipient Account* entities for display to a particular user preparing to initiate a transfer. You can store the *Recipient Account* object for each account in a *data-account* attribute like this:

    ```
    <select id="select-recipient-account" class="form-select">
      <option value="0" selected="">Choose a recipient account</option>
      <option value="fad2ea5a-4738-4057-aaa9-98dd294620cb" data-account="{...}">Alpha Flight Account 1</option>
      <option value="24bb4cd6-dd59-42f4-95e9-c381bd6aa12a" data-account="{...}">Alpha Flight Account 2</option>
    </select>
    ```

    Or, if you just store the *recipientAccountId*, then, when the user makes a selection, you can call [Get Recipient Account](/reference/getrecipientaccount) if needed. 


1. Call [Execute Transfer](/reference/executetransfer).