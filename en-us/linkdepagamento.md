# Payment Link <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for registering and query Payment Links

## New Payment Link
---
<span class="verb httpPOST">POST</span> ***/service/resources/payment_links***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters
> The parameters below must be sent in JSON format

|  Attribute  |  Type  |  Description  |  Mandatory  |  Size  |
|-|-|-|-|-|
|   external_code  |   string  |  Exclusive Payment Link code, defined by the user  |  yes  |   50  |
|   amount  |   numeric  |  Payment Link fixed value, in case of the Payment Link has no defined value, you can send 0 (zero) or do not send this field  |  no  |   -  |
|   **[buttons]**  |   **Object**  |  Preset value buttons data  |  no  |   -  |
|   buttons.enable  |   boolean  |  Flag to activate (true) or not (false) the Preset buttons  |  no  |   -  |
|   buttons.one  |   numeric  |  First preset button value  |  no  |   -  |
|   buttons.two  |   numeric  |  Second preset button value  |  no  |   -  |
|   buttons.three  |   numeric  |  Third preset button value  |  no  |   -  |
|   description  |   string  |  Payment Link description is the main link's message, here you can detail.  |  yes  |   1024  |
|   header  |   string  |  Card's header text from the Payment Link.  |  no  |   30  |
|   subHeader  |   string  | Card's header sub-text from the Payment Link. |  no  |   60  |
|   expireAt  |   datetime  |  Expiration link date (Y-m-d H:i:s)  |  yes  |   -  |
|   **[checkout_settings]**  |   **Object**  |   Checkout Data  |   no  |   - |
|   checkout_settings.max_installments  |   numeric  |  Maximum Installments Value  |   no  |  `1` ~ `12`  |
|   checkout_settings.interest_free_installments  |   numeric  |  Maximum Interest Free Installments  |   no  |  `1` ~ `12`  |
|   checkout_settings.min_installment_value  |   numeric  |  Minimum Installment Amount |   no  |   -  |
|   checkout_settings.interest  |   numeric  |  Interest Amount  |   no  |  `0.00` ~ `100.00`  |
|   checkout_settings.payment_method  |  string  |  Enabled Payment Methods  |  no  |  `all`, `creditcard`, `boleto`, `transfer`  |

### Example of Content to be Sent
Check out in the example below the content that you can send on request body.

```json
{
	"external_code": "127",
	"amount": 0,
	"buttons": {
		"enable": true,
		"one": 10.00,
		"two": 25.00,
		"three": 100.00
	},
	"description": "LINK DE PAGAMENTO SUPER BACANA",
	"header": "DOAÇÃO AVULSA",
	"subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
	"expireAt": "2020-12-25 23:59:59"
}
```

### Return Status
| Code | Description                                                  |
| ---- | ------------------------------------------------------------ |
| 401  | Not authenticated. Check if Basic Authentication keys had been correctly informed. |
| 403  | Not authorized. Your account has no permission to complete this action. |
| 406  | Not accepted. Some sent data can be invalid, verify the data sent on request body. |
| 201  | Resource created.                                            |

#### In case of success

```json
{
  "id": 1000,
  "resource": "payment_links",
  "attributes": {
    "uuid": "15e6d09b-704e-4a41-b1b5-ee8620686f5c",
    "external_code": "130",
    "amount": 0,
    "description": "LINK DE PAGAMENTO SUPER BACANA",
    "buttons": {
      "enable": false,
      "one": 0,
      "two": 0,
      "three": 0
    },
    "header": "Doe Agora",
    "subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
    "expires_at": "2020-12-30 23:00:00",
    "created_at": "2020-09-03 15:52:24",
    "updated_at": "2020-09-03 15:52:24"
  },
  "checkout_settings": {
    "max_installments": 12,
    "interest_free_installments": 12,
    "min_installment_value": 0.00,
    "interest": 0.00,
    "payment_method": "all"
  },
  "links": {
    "payment": "http://api.pluscommercebr.com.br/link?t=15e6d09b-704e-4a41-b1b5-ee8620686f5c"
  }
}
```

#### In case of failure

```json
{
  "code": "406",
  "message": {
    "description": [
      "Description is required",
      "Description must be at least 10 characters long"
    ]
  },
  "resource": "store.payment_link"
}
```

## Query Payment Link

---

> You can list all transactions from specific Payment Link

<span class="verb httpGET">GET</span> ***/service/resources/payment_links?external_code={external_code}***

---

### Query Params

|   Attribute  |   Type   |   Description                |
|-------------|----------|----------------------------|
|   external_code    |   string  |   Exclusive Payment Link code, defined by the user  |
|   id    |   numeric  |   Payment Link ID  |

#### In case of success

```json
{
  "id": 1000,
  "resource": "payment_links",
  "attributes": {
    "uuid": "15e6d09b-704e-4a41-b1b5-ee8620686f5c",
    "external_code": "130",
    "amount": 0,
    "description": "LINK DE PAGAMENTO SUPER BACANA",
    "buttons": {
      "enable": false,
      "one": 0,
      "two": 0,
      "three": 0
    },
    "header": "Doe Agora",
    "subHeader": "Doação destinada a campanha dos meninos desabrigados do Vale do Ribeira",
    "expires_at": "2020-12-30 23:00:00",
    "created_at": "2020-09-03 15:52:24",
    "updated_at": "2020-09-03 15:52:24"
  },
  "links": {
    "payment": "http://api.pluscommercebr.com.br/link?t=15e6d09b-704e-4a41-b1b5-ee8620686f5c"
  },
  "transactions": [
    {
      "id": 1023021,
      "uuid": "bfce317932ecf18f03891b43b6969b13",
      "resource": "transactions",
      "attributes": {
        "seller_id": "",
        "order_id": "20210208105102",
        "amount": 100,
        "installments": 1,
        "tid": "20210217114556769",
        "authorization_id": "",
        "status": {
          "code": 2,
          "message": "WAITING PAYMENT"
        },
        "method": "boletosimulado",
        "captured_amount": 0,
        "captured_at": "0000-00-00 00:00:00",
        "acquirer": {
          "name": "simulated",
          "message": "Boleto impresso",
          "code": ""
        },
        "created_at": "2021-02-17 11:45:56",
        "updated_at": "2021-02-17 11:45:56"
      }
    }
  ]
}
```