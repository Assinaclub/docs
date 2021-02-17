# Charge<!-- {docsify-ignore-all} -->

## Overview

> Service responsible for register and query Charges.

## New Charge
---
<span class="verb httpPOST">POST</span> ***/service/resources/charges***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parameters
> The parameters below must be sent in JSON format.

|  Attribute  |  Type  |  Description  |  Mandatory  |  Size  |
|-|-|-|-|-|
|   amount  |   numeric  |  Charge value  |  Yes  |   -  |
|   description  |   string  |  Charge description  |  Yes  |   1 - 1000  |
|   due_date  |   date  |  Charge due date  |  Yes  |  Y-m-d or d/m/Y  |
|   frequency  |   integer  |  Charge frequency (depends on interval value)  |  No  |   1 - 12  |
|   interval  |   string  |  Charge Interval (daily, weekly, monthly)  |  no  |  `day`, `week` or `month`  |
|   type  |   string  |  Charge type (Normal or Recurring). Standard: Normal  |  no  |  `normal` or `recurring`  |
|   last_charge_date  |   date  |  Last charge date  |   no  |   Y-m-d ou d/m/Y  |
|   callback_url  |   string  |  In case of payment callback URL  |   no  |   255  |
|   auto_debit  |   boolean  |  Active (true) or not (false) automatic debit. Only for Credit Card payments  |   no  |   -  |
|   installments  |   integer  |  Early (previous) installments from the charge  |   no  |   1 - 48  |
|   is_active  |   boolean  |  Active (true) or not (false) a Charge  |   no  |   -  |
|   **[customer]**  |   **Object**  |  Customer data  |  yes  |   -  |
|   customer.name  |   string  |  Customer name  |  yes  |   100  |
|   customer.cpf_cnpj  |   string  |  Customer CPF or CNPJ  |  no  |  CPF or CNPJ  |
|   customer.phone  |   string  |  Customer's phone or cellphone number  |  no  |   10 - 11  |
|   customer.email  |   string  |  Customer's email  |  no  |   50  |

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
    "amount": 150.00,
    "description": "Cobrança referente a negociação de débito pendente na Empresa X",
    "due_date": "30/10/2020",
    "customer": {
        "name": "Maria Francisca",
        "cpf_cnpj": "852.391.120-02",
        "email": "maria@mail.me",
        "phone": "11 99999-9999"
    },
    "callback_url": "https://api.ipag.test/retorno_charge"
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
  "id": 100000,
  "resource": "charges",
  "attributes": {
    "is_active": true,
    "status": "pending",
    "amount": 150,
    "type": "normal",
    "frequency": 1,
    "interval": "month",
    "description": "Cobrança referente a negociação de débito pendente na Empresa X",
    "starting_date": "2020-10-30",
    "closing_date": "",
    "auto_debit": false,
    "callback_url": "https://api.ipag.test/retorno_charge",
    "created_at": "2020-09-08 14:09:14",
    "updated_at": "2020-09-08 14:09:14",
    "customer": {
      "id": 23799,
      "resource": "customers",
      "attributes": {
        "is_active": true,
        "name": "Maria Francisca",
        "cpf_cnpj": "852.391.120-02",
        "email": "maria@mail.me",
        "phone": "11 99999-9999",
        "created_at": "2020-09-08",
        "updated_at": "2020-09-08"
      }
    },
    "installments": {
      "data": [
        {
          "number": 1,
          "due_date": "2020-10-30",
          "amount": 150,
          "paid_amount": 0,
          "discount": 0,
          "payment_date": null,
          "description": "Cobrança referente a negociação de débito pendente na Empresa X",
          "payment": null
        }
      ]
    },
    "links": {
      "payment": "https://api.ipag.com.br/vpos?billing=0771fc6f0f4b1d7d1bb73bbbe14e0e31"
    }
  }
}
```

#### In case of failure

```json
{
  "code": "406",
  "message": {
    "description": [
      "Description is required"
    ]
  },
  "resource": "resources.charge.create"
}
```

## Charge Update
<span class="verb httpPUT">PUT</span> ***/service/resources/charges?id={id}***

### Parameters
> The parameters below must be sent in JSON format

|  Attribute  |  Type  |  Description  |  Mandatory  |  Size  |
|-|-|-|-|-|
|   amount  |   numeric  |  Charge Value  |  yes  |   -  |
|   description  |   string  |  Charge Description  |  yes  |   1 - 1000  |
|   due_date  |   date  |  First Charge Installment due date.  |  yes  |  Y-m-d or d/m/Y  |
|   last_charge_date  |   date  |  Last Charge date  |  no  |  Y-m-d or d/m/Y  |
|   callback_url  |   string  |  In case of payment, callback URL  |  no  |   255  |
|   auto_debit  |   boolean  | Active (true) or not (false) automatic debit. Only for Credit Card payments  |  no  |   -  |
|   is_active  |   boolean  | Active (true) or not (false) a Charge  |  no  |   -  |

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
    "is_active": false
}
```

## Charge Query
<span class="verb httpGET">GET</span> ***/service/resources/charges?id={id}***

## List Charges
<span class="verb httpGET">GET</span> ***/service/resources/charges***