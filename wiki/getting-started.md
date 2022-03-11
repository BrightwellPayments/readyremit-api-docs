---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

# Introduction

This website describes the ReadyRemit Cloud Service which performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors).

DIAGRAM

[Fintech](https://en.wikipedia.org/wiki/Financial_technology) applications access the service via the [ReadyRemit REST API](https://documenter.getpostman.com/view/8773841/UVksNEt7).

## How to sign up

To become a ReadyRemit early adopter, browse to the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact page</a>, scroll to the *Get in touch* form, type your name and email, choose ReadyRemit from the dropdown menu, add a comment, and click Submit. Once you sign up, Brightwell provides you with the following:

1. A unique set of credentials for each of your ReadyRemit client applications. These credentials consist of a `client_id` and a `client_secret`. Each client application uses these two values obtain an access token.

1. A unique senderId for each sender account you support. Each client application needs to pass a senderId to the Create Recipient and Get Recipients operations.
