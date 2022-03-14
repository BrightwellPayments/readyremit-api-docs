---
title: Changelog
excerpt: ""
slug: change-log
category: 6202c91258ac9600635fb56b
---

# ReadyRemit v1

### ReadyRemit v1: Sender operations

The ReadyRemit service supports recipient and sender entities. However, while the ReadyRemit REST API includes operations for dynamically creating, getting, updating, and deleting recipient entities, it does not yet include the same operations for sender entities:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-senders-recipients.png" width=700 loading="lazy"></div>

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

### ReadyRemit v1: Field Types

The fields necessary for building user-facing forms to collect recipient information (and, subsequently, to `POST` recipient and recipient-account data to ReadyRemit) vary depending on country, currency, transfer method, and recipient type. For example, the fields necessary to define a recipient with a bank account in India (Indian Rupee) differ from those necessary to define a recipient in the Philippines (Philippine Peso) expecting a cash remittance:

<div style="margin-top:24px;margin-bottom:24px!important;"><img src="https://raw.githubusercontent.com/hagenhaus/readyremit-images/master/readyremit-variable-fields.png" width=600 loading="lazy"></div>

To accommodate, ReadyRemit provides two operations, *Get Recipient Fields* and *Get Recipient Account Fields*, that return response bodies containing sets of field definitions over which client applications iterate to build user-facing forms. Here is an example:

``` json
{
  "fieldSets": [
    {
      "fieldSetId": "PERSONAL_INFORMATION",
      "fieldSetName": "Personal information",
      "fields": [
        {"fieldType": "PHONE_NUMBER", "fieldId": "PHONE_NUMBER", "name": "Phone Number", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "FIRST_NAME", "name": "First Name", "isRequired": true },
        {"fieldType": "TEXT", "fieldId": "EMAIL", "name": "Email", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "NATIONALITY", "name": "Nationality", "isRequired": false },
        {"fieldType": "TEXT", "fieldId": "LAST_NAME", "name": "Last Name", "isRequired": true },
        {"fieldType": "TEXT", "fieldId": "DATE_OF_BIRTH", "name": "Date of Birth", "isRequired": false }
      ]
    },
    {
      "fieldSetId": "...",
      "fieldSetName": "...",
      "fields": [
        {"fieldType": "...", "fieldId": "...", "name": "...", "isRequired": false },
        {"fieldType": "...", "fieldId": "...", "name": "...", "isRequired": false }
      ]
    }
  ]
}
```

Supported `fieldType` properties include the following:

* `DATE`
* `DROPDOWN`
* `PHONE_NUMBER`
* `TEXT`