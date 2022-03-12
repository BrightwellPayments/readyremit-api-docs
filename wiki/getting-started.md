---
title: Getting Started
excerpt: ""
slug: getting-started
category: 6202c91258ac9600635fb56b
---

This website describes the ReadyRemit Cloud Service, and explains to [fintech](https://en.wikipedia.org/wiki/Financial_technology) application developers how to programmatically access the service via the ReadyRemit REST API. The ReadyRemit Service performs [remittances](https://en.wikipedia.org/wiki/Remittance) on cross-border [corridors](https://remittanceprices.worldbank.org/en/countrycorridors):

<p><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-map.png" width=700 height=393 loading="lazy"></p>

# Use Cases

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

# Sending money

The following diagram illustrates the steps you might present to your end users:

<p><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-workflow.png" width=700 height=512 loading="lazy"></p>

To modify your fintech application to send money via the ReadyRemit Service, complete the following steps:

1. Contact Brightwell via the *Get in touch* form on the <a href="https://brightwell.com/contact-us/" target="_blank">Brightwell Contact</a> page. Fill in your name and email, mention *ReadyRemit* in the comment, and click Submit. Once you sign up, Brightwell will provide you with the following:

    * A *client_id* and a *client_secret* (required for authentication) for each of your ReadyRemit client applications. 
    * A unique *senderId* (required by certain API operations) for each sender account you register.
    * Access to the Postman ReadyRemit collection.

1. Peruse the <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7" target="_blank">ReadyRemit Postman Documentation Site</a>.

1. In the ReadyRemit Postman Collection, call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#231a6946-f65e-4d25-bb45-8192da72177e" target="_blank">Create Access Token</a> to obtain an access token required by all other ReadyRemit REST API operations.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#8f01eb59-3eb5-4eea-8e5a-f7f5e97e4d36" target="_blank">Get Countries and Currencies</a> to get a list of the recipient countries/currencies you support.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#0b582301-8de2-4610-88ba-df0fad04f024" target="_blank">Get Quote</a> to return the recipient amount (based on the sender amount and the exchange rate) and fees.

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#4ec3357f-a4a5-4f53-aca8-1d2c7049a636" target="_blank">Get Recipient Fields</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#7ecc57ba-7c37-49ee-b333-b273402d455a" target="_blank">Create Recipient</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#c0c343a1-9a82-48ff-81a5-4a8e5397e503" target="_blank">Get Banks</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#0baa42f9-245f-4d1f-8fe8-b1cf795e851d" target="_blank">Get Bank Branches</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#653ea7bf-e34b-4272-8d49-8d84ba7a7d34" target="_blank">Get Recipient Account Fields</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#b8d89873-5827-456e-a2d7-b94f6de7b078" target="_blank">Create Recipient Account</a> ...

1. Call <a href="https://documenter.getpostman.com/view/8773841/UVksNEt7#a8d5f9cd-9f84-41c7-8fd4-b9356559c35f" target="_blank">Execute Transfer</a> ...
