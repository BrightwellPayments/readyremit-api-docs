---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

# Introduction

This website describes the ReadyRemit Cloud Service which performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors).

DIAGRAM

[Fintech](https://en.wikipedia.org/wiki/Financial_technology) applications access the service via the [ReadyRemit REST API](https://documenter.getpostman.com/view/8773841/UVksNEt7). To become a ReadyRemit early adopter, browse to the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page, scroll to the *Get in touch* form, type your name and email, choose *ReadyRemit* from the dropdown menu, add a comment, and click Submit. Once you sign up, Brightwell provides you with the following:

1. A unique set of credentials for each of your ReadyRemit client applications. These credentials consist of a *client_id* and a *client_secret*. Each client application passes these two values to <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#231a6946-f65e-4d25-bb45-8192da72177e" target="_blank">Create Access Token</a> which returns an access token. All other ReadyRemit API operations required this access token.

1. A unique *senderId* for each sender account you support. <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#7ecc57ba-7c37-49ee-b333-b273402d455a" target="_blank">Create Recipient</a> and <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#307570a9-6f20-4d53-bb02-ed826cce5473" target="_blank">Get Recipients</a> require a *senderId*.

# ReadyRemit REST API

