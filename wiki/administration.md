---
title: Administration
excerpt: ""
slug: administration
category: 6202c91258ac9600635fb56b
---

This page (not meant for publication) contains instructions for site administrators. 

### How to update schema.yaml

1. Upload schema.yaml to https://app.swaggerhub.com/.

1. Edit and test the file in the SwaggerHub environment.

1. Click *Export* > *Download API* > *YAML Resolved* to export the file. I name it *schema.yaml*.

1. Open the file with a text editor, replace *title: ReadyRemit* with *title: API Reference*, and save.

1. Push the new file to the [readyremit-api-docs](https://github.com/BrightwellPayments/readyremit-api-docs) GitHub repository.

### How to import schema.yaml into Postman

1. Install [openapi-to-postmanv2](https://www.npmjs.com/package/openapi-to-postmanv2) in your Node.js environment on your computer.

    ```
    $ npm i -g openapi-to-postmanv2
    ```

1. Convert *schema.yaml* (Postman) collection.json file:

    ```
    $ openapi2postmanv2 -s schema.yaml -o readyremit-collection.json -p -O folderStrategy=Tags,includeAuthInfoInExample=false
    ```

1. Import the new file into Postman.
