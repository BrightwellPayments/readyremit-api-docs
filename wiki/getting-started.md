---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

This website describes the ReadyRemit Cloud Service, and explains to [fintech](https://en.wikipedia.org/wiki/Financial_technology) application developers how to programmatically access the service via the ReadyRemit REST API. The ReadyRemit Service performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors):

<p><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 height=393 loading="lazy"></p>

# Application Workflow

ReadyRemit REST API operations enable you to create the following workflow for your end users:

<p><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-workflow.png" width=700 height=512 loading="lazy"></p>

# Directions

To modify your fintech application to leverage the ReadyRemit Service, complete the following steps:

1. Contact Brightwell via the *Get in touch* form on the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page. Fill in your name and email, mention *ReadyRemit* in the comment, and click Submit. Once you sign up, Brightwell will provide you with the following:

    * A *client_id* and a *client_secret* (required for authentication) for each of your ReadyRemit client applications. 
    * A unique *senderId* (required by certain API operations) for each sender account you support.
    * Access to the Postman ReadyRemit collection.

1. Peruse the <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7" target="_blank">ReadyRemit Postman Documentation Site</a>.

1. In the ReadyRemit Postman Collection, call Get Access Token to obtain the access token required by all other API operations.

1. Call Get Countries and Currencies to get possible recipient countries and currencies.

1. Call Get Quote to return the destination amount and fees.

1. Call Get Sender Fields.

1. Call Create Sender.

1. Call Get Recipient Fields.

1. Call Create Recipient.

1. Call Get Recipient Account Fields.

1. Call Get Banks.

1. Call Get Bank Branches.

1. Call Create Recipient Account.

1. Call Execute Transfer.
