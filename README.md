# ![LOGO](logo.png) Accounting – Documents **flow**ground Connector

## Description

A generated **flow**ground connector for the Accounting – Documents API (version 1.0).

Generated from: https://api.datev.de/<br/>
Generated at: 2019-05-10T13:37:11+03:00

## API Description

context: accounting/v1

The individual API services are described both functionally and technically. Wherever possible, examples are used to illustrate the calls and responses. All services only support the JSON format. The order of the return values is not set and may deviate from the order specified in this document. Return values with a value of “null” will not be returned.

## Authorization

Supported authorization schemes:
- OAuth2
- API Key- OAuth2

For OAuth 2.0 you need to specify OAuth Client credentials as environment variables in the connector repository:
* `OAUTH_CLIENT_ID` - your OAuth client id
* `OAUTH_CLIENT_SECRET` - your OAuth client secret

## Actions

### Returns a list of clients that are accessible for the user.

> Returns a list of all clients to which the user has access. The user is only familiar with the client name, the client number, and the consultant number. For all subsequent requests concerning data transfer to DATEV, the ID of the client is necessary. The ID is a technical object for the communication between the third-party solution and the DATEV data center. The ID is not to be displayed to the end user. <br/>
> <br/>
> **Please note:** Data transfer to the DATEV data center is only possible if the user has access to the client. The third-party solution must ensure this by checking that the client number and consultant number exists in the list of clients.<br/>
> <br/>
> **Example: The user has access to one client**<br/>
> <br/>
>     [{<br/>
>         "client_number": 100,<br/>
>         "consultant_number": 29098, <br/>
>         "id": "29098-100",<br/>
>         "name": "Muster GmbH 1"<br/>
>     }]<br/>
>     <br/>
>     <br/>
> **Example: The user has access to two clients** <br/>
>     <br/>
>     <br/>
>     [{<br/>
>         "client_number":100,<br/>
>         "consultant_number":29098, <br/>
>         "id":"29098-100",<br/>
>         "name":"Muster GmbH 1"}, <br/>
>       {<br/>
>         "client_number":101,<br/>
>         "consultant_number":29098, <br/>
>         "id":"29098-101",<br/>
>         "name":"Muster GmbH 2"<br/>
>           <br/>
>       }]

*Tags:* `Get clients and basic data about a client`

### Returns a list of document types for a given client.

> Returns a list of document types for a given client. The user is normally only familiar with the name of the document type. The document type is determined by the properties category and debit_credit_identifier. Further processing in DATEV Unternehmen online depends on these properties. For example, a document type with the category "invoice_received" is processed as an incoming invoice. <br/>
> <br/>
> The document type in DATEV applications is called "Belegtyp". A client can have a lot of different document types. There are document types with special restrictions - document types with the category "personnel_documents" and "travel_expense_documents". This means that the user's DATEV authentication method needs to be configured with special rights.<br/>
> <br/>
> **Please note:** If the returned list is empty then the client has no document types. Normally, a client has at least four document types.<br/>
>  <br/>
> <br/>
>  **Get list of document types**<br/>
>  <br/>
> <br/>
> ```json<br/>
> HTTP/1.1 200 OK<br/>
> Content-Encoding: gzip<br/>
> Transfer-Encoding: chunked<br/>
> <br/>
> [<br/>
> {<br/>
> "name":"Rechnungseingang",<br/>
> "category":"invoices_received","debit_credit_identifier":"debit"<br/>
> },<br/>
> {<br/>
> "name":"Rechnungsausgang",<br/>
> "category":"outgoing_invoices",<br/>
> "debit_credit_identifier":"credit"<br/>
> },<br/>
> {<br/>
> "name":"Kasse",<br/>
> "category":"other_documents"<br/>
> },<br/>
> {<br/>
> "name":"Sonstige",<br/>
> "category":"other_documents"<br/>
> }<br/>
> ]<br/>
> <br/>
> ```

*Tags:* `Get clients and basic data about a client`

#### Input Parameters
* `client-id` - _required_ - ID that identifies the client.

### Transfers a document

> A new accounting document will be transferred to Belege online in DATEV Unternehmen online. Each accounting document is transferred in a multipart object as a single file with optional metadata (document type and note). The content-type indicated in the multipart object has to correspond to the file. The accepted content-types and the accepted file size are determined by Belege online. Exception: ZIP files are not supported.<br/>
> <br/>
> It is recommended that the user chooses the document type for each accounting document, as the document type affects further processing by DATEV. The chosen document type has to be existent at the given client. Please check the list of document types for a given client. The user should also have the possibility to choose "no document type". <br/>
> <br/>
> If the value of the document_type is empty, then the parameter will be ignored and will not be returned in the response. In this case, and if the parameter document_type is not used, the term "ohne Belegtyp" (without document type) is displayed in the DATEV applications.<br/>
> <br/>
> **Please note:** <br/>
> It is recommended that file names are encoded in UTF8. Unicode characters have to be precomposed characters; in other words, each special character, such as an umlaut (a, u, o), has to be a single code point. Decomposed characters are not supported. This applies to both the file name and the metadata.<br/>
> <br/>
> Large files require more storage space and can slow down load times within DATEV Unternehmen online and the DATEV Rechnungswesen-Programm. We recommend that the user is informed whenever the file size exceeds 500 kB.<br/>
>  <br/>
> **Example:Document has been imported successfully:**<br/>
>  <br/>
> <br/>
> ```json<br/>
> HTTP/1.1 201 Processed<br/>
> Content-Encoding: gzip<br/>
> Transfer-Encoding: chunked<br/>
> <br/>
> {<br/>
> "files":[{<br/>
> "name":"Test-File.pdf",<br/>
> "size":"125742"<br/>
> }],<br/>
> "document_type":"Rechnungseingang",<br/>
> "note":"Beleg von Beispiel GmbH"<br/>
> }<br/>
> <br/>
> ```

*Tags:* `Transfer a document`

#### Input Parameters
* `client-id` - _required_ - ID that identifies the client.

### Transfers a document with an ID

> A new accounting document with the ID of the document in the format of a GUID will be transferred to Belege online in DATEV Unternehmen online.<br/>
> <br/>
> **Please note:** If the ID of the document already exists in Belege online, then the import will be aborted with an error.<br/>
> <br/>
> Each accounting document is transferred in a multipart object as a single file with optional metadata (document type and note). The content-type indicated in the multipart object has to correspond to the file. The accepted content-types and the accepted file size are determined by Belege online. Exception: ZIP files are not supported.<br/>
> <br/>
> It is recommended that the user chooses the document type for each accounting document, as the document type affects further processing by DATEV. The chosen document type has to be existent at the given client. Please check the list of document types for a given client. The user should also have the possibility to choose "no document type".<br/>
> <br/>
> If the value of document_type is empty, then the parameter will be ignored and will not be returned in the response. In this case, and if the parameter document_type is not used, the term "ohne Belegtyp" (without document type) is displayed in the DATEV applications.<br/>
> <br/>
> **Please note:** <br/>
> It is recommended that file names are encoded in UTF8. Unicode characters have to be precomposed characters; in other words, each special character, such as an umlaut (a, u, o), has to be a single code point. Decomposed characters are not supported. This applies to both the file name and the metadata.<br/>
> <br/>
> Large files require more storage space and can slow down load times within DATEV Unternehmen online and the DATEV Rechnungswesen-Programm. We recommend that the user is informed whenever the file size exceeds 500 kB.<br/>
> <br/>
> <br/>
> **Example: Document has been imported succesfully:**<br/>
> <br/>
> <br/>
> ```json<br/>
> HTTP/1.1 201 Processed<br/>
> Content-Encoding: gzip<br/>
> Transfer-Encoding: chunked<br/>
> <br/>
> {<br/>
> "files":[{<br/>
> "name":"Test-File.pdf",<br/>
> "size":"125742"<br/>
> }],<br/>
> "document_type":"Rechnungseingang",<br/>
> "note":"Beleg von Beispiel GmbH"<br/>
> }<br/>
> <br/>
> ```

*Tags:* `Transfer a document`

#### Input Parameters
* `client-id` - _required_ - ID that identifies the client.
* `document-id` - _required_ - ID that identifies the document.

## License

**flow**ground :- Telekom iPaaS / accounting-documents-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
