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

You obtain for your service a *senderId* when you sign up as a ReadyRemit client. You can store the *senderId* in an environment file (read at startup) or in your database (especially if you need to restrict access to it). 

Each of your end users permitted to receive remittances needs a *recipientId*. Your service obtains each *recipientId* by calling [Create Recipient](/reference/createrecipient), and can store them in your database either in your *Users* table (as shown in the diagram) or in a related table.

## Using identifiers

This section explains how your service can use the identifers illustrated in the diagram to transfer money. Because the implementation of this use case is evolving, details may change.

### senderId

Your service uses the *senderId* to call [Create Recipient](/reference/createrecipient) and [Get Recipients](/reference/getrecipients): 

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-sender-recipient-6.png" height=160 loading="lazy"></div>

When this use case is fully implemented, your service may also use your *senderId* to get or update your ReadyRemit Sender entity, create  *SenderAccount* entities, and get an array of *SenderAccount* entities:

IMAGE HERE

### recipientId

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-recipient-recipientaccount-6.png" height=280 loading="lazy"></div>

### recipientAccountId

# Your users are the senders

<span style="color:red;">Note: The ReadyRemit Team is not targeting this use case yet.</span>

In this use case, ReadyRemit represents each of your end users with a *Sender* entity and one or more *Sender Account* entities, and ReadyRemit represents each of your end user's recipients with a *Recipient* entity and one or more *Recipient Account* entities. Your database may not have any information about your users' recipients, so the ReadyRemit *Recipient* entity provides your service with a convenient place to store general profile information about these out-of-database recipients.

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-users-are-the-senders-4.png" width=800 loading="lazy"></div>

