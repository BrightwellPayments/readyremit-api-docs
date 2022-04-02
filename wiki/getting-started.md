---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit, a cloud service that performs cross-border [remittances](https://en.wikipedia.org/wiki/Remittance) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=600 loading="lazy"></div>

# Overview

ReadyRemit leverages core [Brightwell](https://brightwell.com/) expertise, and makes it available as a generic remittance service to financial technology companies targeting various use cases including [Your service as the sender](#your-service-as-the-sender) and [Your users as the senders](#your-users-as-the-senders).

Currently, ReadyRemit is suitable for early adopters targeting [Your service as the sender](#your-service-as-the-sender) where your service (1) pushes money to end users, or (2) allows end users to pull money from your service.

Click [Contact Us](/docs/contact-us) to become an early adopter.

## Interfaces

ReadyRemit offers a [REST API](/reference). Language-specific SDKs are on the roadmap.

## Sandbox

ReadyRemit is available in a [sandbox](https://sandbox-api.readyremit.com) environment where you can call API operations from the [API Reference](/reference), [SwaggerHub](https://app.swaggerhub.com/), [Postman](https://www.postman.com/), or your service as you experiment with integration.

## Versioning

The current ReadyRemit version is ReadyRemit v1. The `v1` corresponds to the `v1` in REST API endpoints like this:

```
/v1/recipients
```

In general, a version number like `v1` means that, although the ReadyRemit service may experience several subsequent releases to support new endpoints, query parameters, status codes, etc., the API will remain backward compatible. However, during the ReadyRemit early-adoption phase (Spring 2022), in response to suggestions from early adopters, `v1` may experience improvements that are not backward compatible.

## Releases

Every Friday morning at 8 am ET (starting April 1, 2022), the ReadyRemit Team pushes a new ReadyRemit release to the [sandbox](https://sandbox-api.readyremit.com) environment and publishes a corresponding [release note](https://developer.readyremit.com/changelog).

# Your service as the sender

In this use case, ReadyRemit represents your service with a *Sender* record and one or more *Sender Account* records, and ReadyRemit represents each of your end users with a *Recipient* record and one or more *Recipient Account* records:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-service-as-the-sender.png" width=800 loading="lazy"></div>



The following sections explain one way to implement this use case in a Node.js environment. Implementations in other environments are similar.

## Authenticate

1. Obtain values from Brightwell for the following properties:

    ```
    {
      "client_id": "",
      "client_secret": "",
      "audience": "",
      "grant_type": ""
    }
    ```

## Establish the sender

1. Supply Brightwell with sender information. Brightwell uses the information to create a ReadyRemit *Sender* record and a *Sender Account* record for your client service:

    <div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-sender-sender-account-2.png" width=400 loading="lazy"></div>

    And, Brightwell supplies you with the corresponding *senderId* for use in certain API operations:

    ```
    {
      "senderId": "12300000-0000-0000-0000-000000000321"
    }
    ```

1. I will finish this workflow by Monday April 4, 2022 08:00:00.

# Your users as the senders

<span style="color:red;">Note: The ReadyRemit Team is not targeting this use case yet.</span>

In this use case, ReadyRemit represents each of your end users with a *Sender* record and one or more *Sender Account* records, and ReadyRemit represents each of your end user's recipients with a *Recipient* record and one or more *Recipient Account* records. Your database may not have any information about your users' recipients, so the ReadyRemit *Recipient* record provides your service with a convenient place to store general profile information about these out-of-database recipients.

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-your-users-as-the-senders.png" width=800 loading="lazy"></div>

