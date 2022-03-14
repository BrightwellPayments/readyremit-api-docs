---
title: Limitations
excerpt: ""
slug: limitations
category: 6202c91258ac9600635fb56b
---

# Version 1.0

1. The ReadyRemit service supports recipient and sender entities. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient entities, it does not yet include the same operations for sender entities:

    <div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

    So, for Version 1, ReadyRemit early adopters manually submit information about sender entities to Brightwell in exchange for *senderIds* required by various REST API operations (e.g. [Create Recipient](https://readyremit.readme.io/reference/createrecipient)).

1. Version 1 allows client applications to perform quote calculations based on the send amount, but not on the recipient amount. So, [Get Quote](https://readyremit.readme.io/reference/getquote) ignores the `quoteBy` query parameter. 