---
title: Changelog
excerpt: ""
slug: change-log
category: 6202c91258ac9600635fb56b
---

# ReadyRemit v1

### ReadyRemit v1: Sender operations

The ReadyRemit service supports recipient and sender entities. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient entities, it does not yet include the same operations for sender entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/BrightwellPayments/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

So, early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by various REST API operations (e.g. [Create Recipient](https://readyremit.readme.io/reference/createrecipient)).

### ReadyRemit v1: Quote by sender

ReadyRemit allows client applications to perform quote calculations based on the send amount, but not on the recipient amount. Therefore, for example, [Get Quote](https://readyremit.readme.io/reference/getquote) ignores the `quoteBy` query parameter.

### ReadyRemit v1: Transfer methods

The ReadyRemit service does not yet support a *Get Transfer Methods* operation. Here are the available values:

* `BANK_ACCOUNT`
* `CASH_PICKUP`

### ReadyRemit v1: Recipient Types

The ReadyRemit service does not yet support a *Get Recipient Types* operation. Here are the available values:

* `MYSELF`
* `SOMEONE_ELSE`
* `BUSINESS`