---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit, a cloud service that performs cross-border [remittances](https://en.wikipedia.org/wiki/Remittance) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=600 loading="lazy"></div>

# Overview

ReadyRemit leverages core [Brightwell](https://brightwell.com/) expertise, and makes it available as a generic remittance service to financial technology companies targeting various use cases including [Your service is the sender](#your-service-is-the-sender) and [Your users are the senders](#your-users-are-the-senders).

Currently, ReadyRemit is suitable for early adopters targeting [Your service is the sender](#your-service-is-the-sender) where your service (1) pushes money to end users, or (2) allows end users to pull money from your service.

Click [Contact Us](/docs/contact-us) to become an early adopter.

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

Every Friday morning at 8 am ET (starting April 1, 2022), the ReadyRemit Team pushes a new ReadyRemit release to the [sandbox](https://sandbox-api.readyremit.com) environment and publishes a corresponding [release note](https://developer.readyremit.com/changelog).

## Authentication

When you sign up to use ReadyRemit, Brightwell creates a configuration for your app in the ReadyRemit account of an authentication service:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-authentication.png" width=700 loading="lazy"></div>

Brightwell also provides you with credentials (i.e. `client_id`, `client_secret`, `audience`, and `grant_type`) which, at start up, your app reads from an environment file, and trades (via [Get Access Token](/reference/getaccesstoken)) for an *access_token* that, subsequently, gives your app access to all the other ReadyRemit REST API operations:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-auth-bearer.png" width=700 loading="lazy"></div>

Note that this authentication workflow is independent of any authentication workflow your app performs for your end users.

# Your service is the sender

For this use case, your service is the sender, and your end users are the recipients. The use case supports transfers initiated by your service (e.g. payroll) and transfers initiated by your end users (e.g. reward redemption). ReadyRemit represents your service with a *Sender* entity and one or more *Sender Account* entities, and ReadyRemit represents each of your end users with a *Recipient* entity and one or more *Recipient Account* entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-service-is-the-sender-4.png" width=800 loading="lazy"></div>

You obtain a *senderId* for your service when you sign up as a ReadyRemit client. You can store the *senderId* in an environment file (read at startup) or in your database (especially if you need to restrict access to it). 

## Using the senderId

Your service uses the *senderId* to call [Create Recipient](/reference/createrecipient) for each end user permitted to receive remittances, storing the returned *recipientIds* in the service's *Users* table (as shown in the diagram) or in a related table:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-pseudocode-alpha-2.png" height=118 loading="lazy"></div>



Each of your end users permitted to receive remittances needs a *recipientId*. Your service obtains each *recipientId* by calling [Create Recipient](/reference/createrecipient), and can store them in your database either in your *Users* table (as shown in the diagram) or in a related table.

 and (optionally) [Get Recipients](/reference/getrecipients). Calling *Get Recipients* is optional because your service will more likely call [Get Recipient](/reference/getrecipient) for each *recipientId* returned by *Create Recipient* and stored in your database.

The implementation of this use case does not yet include operations that target *Sender* or *SenderAccount* entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-pseudocode-beta.png" height=345 loading="lazy"></div>

This is not a critical issue, but it means that, for now, you need to manage *Sender* and *Sender Account* information in your service. *Sender* information includes the ReadyRemit *senderId* and perhaps your company name, address, etc. *Sender Account* information might include account name, account number, bank name, bank number, branch name, branch number, country code, and currency code. If you need more than one *Sender Account*, then you might store this information in your service's own **SenderAccount** database table like this:

|senderAccountId|accountNumber|accountName|bankId|bankName|branchId|branchName|countryIso3Code|currencyIso3Code|
|-|-|-|-|-|-|-|-|-|
|1|829475946|Primary|B0000a|Apex|BR00000a1|Acadia|USA|USD|
|2|510029384|Secondary|B0000a|Apex|BR00000a2|Arches|USA|USD|

Your service needs *Sender Account* information to call [Get Quote](/reference/getquote), [Execute Transfer](/reference/executetransfer), and other operations.

## Using a recipientId

## Using a recipientAccountId

# Your users are the senders

<span style="color:red;">Note: The ReadyRemit Team is not targeting this use case yet.</span>

In this use case, ReadyRemit represents each of your end users with a *Sender* entity and one or more *Sender Account* entities, and ReadyRemit represents each of your end user's recipients with a *Recipient* entity and one or more *Recipient Account* entities. Your database may not have any information about your users' recipients, so the ReadyRemit *Recipient* entity provides your service with a convenient place to store general profile information about these out-of-database recipients.

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-users-are-the-senders-4.png" width=800 loading="lazy"></div>

