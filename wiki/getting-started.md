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

* *client_id*, *client_secret*, *audience*, and *grant_type* values for each application that accesses ReadyRemit. 
* A *senderId* for each sender entity you register with ReadyRemit.

The first set of values are required for authentication.

# Accessing the service

The ReadyRemit service is accessible via a REST API which enables client applications to (1) build relevant forms for end-user input, (2) instantiate receiver and receiver-account objects in ReadyRemit, and (3) make cross-border payments from senders to recipients:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-use-ops.png" width=600 loading="lazy"></div>

The ReadyRemit service supports recipient and sender records. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient records, it does not yet include the same operations for sender records:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

So, early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by some REST API operations.

# API documentation

ReadyRemit REST API documentation resides in an *openapi.yaml* file which feeds (or can feed) applications for viewing and running:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-oas-workflow.png" width=620 loading="lazy"></div>

Currently, in the ReadyRemit Developer Portal, the *openapi.yaml* file is viewable, but not runnable.

ReadyRemit makes available this OpenAPI file and its companion Postman Collection file:

* <a href="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-openapi-v1-20220315.yaml" target="_blank">readyremit-openapi-v1-20220315.yaml</a>

* <a href="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-postman-v1-20220315.json" target="_blank">readyremit-postman-v1-20220315.json</a>

**These files will change frequently during the early-adoption phase. All changes will be recorded in the [Changelog](https://developer.readyremit.com/changelog).**

# Primary workflow

The primary workflow focuses on creating a new recipient record and associated recipient-account record (in ReadyRemit), and sending funds from a previously defined sender account to the new recipient account. Below are the steps.

## Get an access token

ReadyRemit uses the <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow" target="_blank">Client Credentials Flow</a> of the <a href="https://auth0.com/" target="_blank">Auth0</a> Platform for authentication:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth0.png" width=700 loading="lazy"></div>

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the Auth0 ReadyRemit account, and provides you with a *client_id* and a *client_secret* which, at start up, your app reads from an enviroment file, and trades (via an API call to Auth0) for an *access_token* that, subsequently, gives your app access to ReadyRemit REST API operations:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 loading="lazy"></div>

This *access_token* gives your application access to the ReadyRemit service. It is independent of any authentication workflow your app performs for your end users. 

To get an access token, call <a href="https://readyremit.readme.io/reference/getaccesstoken" target="_blank">Get Access Token</a> which returns an `access_token` for use in other API operations:

```
{
  "access_token": "abc",
  "scope": "test",
  "expires_in": 1800,
  "token_type": "Bearer"
}
```

## Get a list of countries

ReadyRemit enables you to populate your user interface with the names and ISO 3-letter codes of countries and (where necessary) currencies:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-countries-and-currencies.png" width=450 loading="lazy"></div>

An [ISO 3-letter country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) and an [ISO 3-letter currency code](https://en.wikipedia.org/wiki/ISO_4217) are factors in determining amounts, fees, and disclosures. 

To obtain countries and currencies, call [Get Countries](https://readyremit.readme.io/reference/getcountries) which returns a list of countries and their currencies. The current sandbox environment returns only one country:

```
[
  {
    "country": {
      "name": "India",
      "iso3Code": "IND"
    },
    "currencies": [
      {
        "name": "Indian rupee",
        "iso3Code": "INR",
        "symbol": "₹",
        "decimalPlaces": 2
      }
    ]
  }
]
```

## Get a quote

Establishing a quote is based on the following values:

|Key|Description|
|-|-|
|`dstCountryIso3Code`|The recipient country obtained in the previous step.|
|`dstCurrencyIso3Code`|The recipient currency obtained in the previous step.|
|`srcCurrencyIso3Code`|Registered when you signed up for the service.|
|`transferMethod`|`BANK_ACCOUNT` or `CASH_PICKUP`|
|`quoteBy`|Not implemented yet.|
|`amount`|Obtain this value from the end user.|

Many applications obtain sender amount and transfer method from the end user:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-sender-amount-and-transfer-method.png" width=450 loading="lazy"></div>

Once you've obtained the information in the table, call [Get Quote](https://readyremit.readme.io/reference/getquote) to obtain the recipient amount, fees, and disclosures. Below is an example.

> Note: The values in this example reflect an obsolete implementation. Eventually, all currency values will be integers. See [Currency](https://developer.readyremit.com/reference/common-concepts#currency).

```
{
  "sendAmount": {
    "value": 1000,
    "currency": {
      "name": "US Dollar",
      "iso3Code": "USD",
      "symbol": "$",
      "decimalPlaces": 2
    }
  },
  "receiveAmount": {
    "value": 74457.6574896,
    "currency": {
      "name": "Indian rupee",
      "iso3Code": "INR",
      "symbol": "₹",
      "decimalPlaces": 2
    }
  },
  "rate": 74.4576574896,
  "adjustments": [
    {
      "label": "Transfer Fee",
      "amount": {
        "value": 8.79,
        "currency": {
          "name": "US Dollar",
          "iso3Code": "USD",
          "symbol": "$",
          "decimalPlaces": 2
        }
      }
    }
  ],
  "totalCost": {
    "value": 1008.79,
    "currency": {
      "name": "US Dollar",
      "iso3Code": "USD",
      "symbol": "$",
      "decimalPlaces": 2
    }
  },
  "disclosures": []
}
```

Once you have this data, you can display the quote to the end user for approval:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-quote-form.png" width=470 loading="lazy"></div>

## Create a recipient

The fields necessary for building a user-facing form to collect recipient information and create recipient records in ReadyRemit vary depending on country, currency, transfer method, and recipient type. 

1. Obtain a recipient type (`MYSELF`, `SOMEONE_ELSE`, `BUSINESS`) from the end user:

    <div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient-type.png" width=220 loading="lazy"></div>

1. Call [Get Recipient Fields](https://readyremit.readme.io/reference/getrecipientfields) to determine appropriate field. Here is an example from the current sandbox environment:

    ```
    {
      "fieldSets": [
        {
          "fieldSetId": "PERSONAL_INFORMATION",
          "fieldSetName": "Personal information",
          "fields": [
            {
              "fieldType": "PHONE_NUMBER",
              "fieldId": "PHONE_NUMBER",
              "name": "Phone Number",
              "isRequired": false
            },
            {
              "fieldType": "TEXT",
              "fieldId": "FIRST_NAME",
              "name": "First Name",
              "isRequired": true
            },
            {
              "fieldType": "TEXT",
              "fieldId": "EMAIL",
              "name": "Email",
              "isRequired": false
            },
            {
              "fieldType": "TEXT",
              "fieldId": "NATIONALITY",
              "name": "Nationality",
              "isRequired": false
            },
            {
              "fieldType": "TEXT",
              "fieldId": "LAST_NAME",
              "name": "Last Name",
              "isRequired": true
            },
            {
              "fieldType": "TEXT",
              "fieldId": "DATE_OF_BIRTH",
              "name": "Date of Birth",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Customer_Division_Designer",
          "fieldSetName": "Customer Division Designer",
          "fields": [
            {
              "fieldType": "TEXT",
              "fieldId": "COMPANY_NAME",
              "name": "Company Name",
              "isRequired": false
            }
          ]
        }
      ]
    }
    ```

1. Iterate over the fields to build and display a form for the end user:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient.png" width=470 loading="lazy"></div>

1. Call [Create Recipient](https://readyremit.readme.io/reference/createrecipient) to create a recipient record in ReadyRemit. The request might resemble this:

    ```
    {
      "dstCountryIso3Code": "IND",
      "dstCurrencyIso3Code": "INR",
      "recipientType": "MYSELF",
      "transferMethod": "BANK_ACCOUNT",
      "senderId": "85add64d-1e36-4acb-84e1-2de9b5644001",
      "fields": [
        {
          "id": "FIRST_NAME",
          "type": "TEXT",
          "value": "Snow"
        },
        {
          "id": "LAST_NAME",
          "type": "TEXT",
          "value": "White"
        }
      ]
    }
    ```

    And, the response might look like this:

    ```
    {
      "recipientId": "4e6e2df5-ee18-451a-b238-dba4b3b3c394",
      "senderId": "85add64d-1e36-4acb-84e1-2de9b5644001",
      "firstName": "Snow",
      "lastName": "White",
      "fields": []
    }
    ```

## Create a recipient account

The fields necessary for building a user-facing form to collect recipient *account* information also vary depending on the characteristics of the remittance corridor. 

1. Call [Get Recipient Account Fields](https://readyremit.readme.io/reference/getrecipientaccountfields). Currently, this operation returns test data:

    ```
    {
      "fieldSets": [
        {
          "fieldSetId": "PERSONAL_INFORMATION",
          "fieldSetName": "Personal information",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "TEXT",
              "fieldId": "LegacyApplicationsAdministrator",
              "name": "District Optimization Analyst",
              "hintText": "Human Paradigm Associate",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Global_Paradigm_Administrator",
          "fieldSetName": "Global Paradigm Administrator",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "CustomerOptimizationTechnician",
              "name": "Central Infrastructure Officer",
              "hintText": "Principal Group Executive",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Internal_Usability_Agent",
          "fieldSetName": "Internal Usability Agent",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "InternalResearchDirector",
              "name": "Product Usability Orchestrator",
              "hintText": "Investor Applications Agent",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Dynamic_Directives_Associate",
          "fieldSetName": "Dynamic Directives Associate",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "SeniorBrandConsultant",
              "name": "Legacy Directives Technician",
              "hintText": "Human Applications Consultant",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Dynamic_Quality_Specialist",
          "fieldSetName": "Dynamic Quality Specialist",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "ProductDataLiaison",
              "name": "Future Applications Specialist",
              "hintText": "Principal Accounts Manager",
              "isRequired": true
            }
          ]
        },
        {
          "fieldSetId": "Forward_Integration_Administrator",
          "fieldSetName": "Forward Integration Administrator",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "PrincipalIntegrationFacilitator",
              "name": "National Mobility Strategist",
              "hintText": "Future Division Officer",
              "isRequired": false
            }
          ]
        },
        {
          "fieldSetId": "Global_Research_Representative",
          "fieldSetName": "Global Research Representative",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "LeadIdentityAdministrator",
              "name": "Central Response Liaison",
              "hintText": "Corporate Accounts Representative",
              "isRequired": true
            }
          ]
        },
        {
          "fieldSetId": "Dynamic_Paradigm_Planner",
          "fieldSetName": "Dynamic Paradigm Planner",
          "fields": [
            {
              "validationErrors": [],
              "fieldType": "UNKNOWN",
              "fieldId": "ForwardCreativeTechnician",
              "name": "Human Brand Architect",
              "hintText": "International Paradigm Agent",
              "isRequired": true
            }
          ]
        }
      ]
    }
    ```

1. Call [Get Banks](https://readyremit.readme.io/reference/getbanks):

    ```
    [
      {
        "id": "IND60",
        "name": "PUNJAB NATIONAL BANK"
      },
      {
        "id": "IND30",
        "name": "DEUTSCHE BANK"
      },
      {
        "id": "IN134",
        "name": "NAGPUR NAGARIK SAHAKARI BANK"
      },
      {
        "id": "IN02",
        "name": "ESAF SMALL FINANCE BANK LTD"
      },
      ...
      ...
    ]
    ```

1. Call [Get Bank Branches](https://readyremit.readme.io/reference/getbankbranches):

    ```
    [
      {
        "id": "PUNB0RTGS00",
        "name": "Rtgs Center"
      },
      {
        "id": "PUNB0368800",
        "name": "Kaushalpuri"
      },
      {
        "id": "PUNB0888200",
        "name": "RAJNAGAR"
      },
      ...
      ...
    ]
    ```

1. Build and display a form based on the fields and bank information:

    <div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient-account.png" width=470 loading="lazy"></div>

1. Call [Create Recipient Account](https://readyremit.readme.io/reference/createrecipientaccount) to create a recipient-account record in ReadyRemit. The request might resemble this:

    ```
    {
        "dstCountryIso3Code": "IND",
        "dstCurrencyIso3Code": "INR",
        "transferMethod": "BANK_ACCOUNT",
        "senderId": "85add64d-1e36-4acb-84e1-2de9b5644001",
        "recipientId": "4e6e2df5-ee18-451a-b238-dba4b3b3c394",
        "name": "Destination Account",
        "accountNumber": "98237498327489273",
        "fields": [
            {
                "id": "bankId",
                "type": "NUMBER",
                "value": "78"
            },
            {
                "id": "branchId",
                "type": "NUMBER",
                "value": "99"
            },
            {
                "id": "accountNumber",
                "type": "TEXT",
                "value": "1234556677"
            }
        ]
    }
    ```

    And, the response might look like this:

    ```
    {
        "recipientAccountId": "faf360e5-5c14-4a98-9766-a77bfc81b0cc",
        "accountNumber": "98237498327489273",
        "fields": [
            {
                "id": "bankId",
                "type": "NUMBER",
                "value": "78"
            },
            {
                "id": "branchId",
                "type": "NUMBER",
                "value": "99"
            },
            {
                "id": "accountNumber",
                "type": "TEXT",
                "value": "1234556677"
            }
        ]
    }
    ```

## Execute the transfer

You should now have all the information needed to execute the transfer:

|Key|Value|
|-|-|
|`dstCountryIso3Code`|sss|
|`dstCurrencyIso3Code`|sss|
|`srcCurrencyIso3Code`|sss|
|`transferMethod`|sss|
|`quoteBy`|sss|
|`amount`|sss|
|`senderId`|sss|
|`recipientId`|sss|
|`recipientAccountId`|sss|
|`purposeOfRemittance`|sss|

Once it is implemented, you can call [Execute Transfer](https://readyremit.readme.io/reference/executetransfer). 