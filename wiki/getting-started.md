---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

Welcome to ReadyRemit v1, a cloud service that performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors) for [fintech](https://en.wikipedia.org/wiki/Financial_technology) applications:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 loading="lazy"></div>

Click [Contact Us](/docs/contact-us) to learn more. 

# About versioning

ReadyRemit v1 is a new service, currently suitable for early adopters. The `v1` is called the *version number*. It corresponds to the `v1` in REST API endpoints like this:

```
/v1/recipients
```

In general, a version number like `v1` means that, although the ReadyRemit service may experience several subsequent releases to support new endpoints, query parameters, status codes, etc., the API will remain backward compatible.

However, during the ReadyRemit early-adoption phase (Spring 2022), in response to suggestions from early adopters, `v1` may experience improvements that are not backward compatible.

# About releases

Every Friday morning at 8 am ET (starting April 1, 2022), the ReadyRemit Team pushes a new ReadyRemit release to the [sandbox](https://sandbox-api.readyremit.com) environment and publishes a corresponding [release note](https://developer.readyremit.com/changelog).

# Use cases

Broadly speaking, ReadyRemit targets two use case:

1. Your service as the sender
1. Your users as the senders

## Your service as the sender

In this case, ReadyRemit represents your service with a *Sender* record and one or more *Sender Account* records, and ReadyRemit represents each of your end users with a *Recipient* record and one or more *Recipient Account* records.

## Your users as the senders

In this case, ReadyRemit represents each of your end users with a *Sender* record and one or more *Sender Account* records, and ReadyRemit represents each of your end user's recipients with a *Recipient* record and one or more *Recipient Account* records.

