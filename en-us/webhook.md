# Webhook <!-- {docsify-ignore-all} -->

## Overview

> Service responsible for store, consult and delete Webhooks.

!> Webhook é a forma de recebimento de informações, que são passadas quando um evento acontece.
!> Webhook its a way of receiving information, that are passed when an Event is thrown.

## New Webhook
---
<span class="verb httpPOST">POST</span> ***/service/resources/webhooks***

---

### Headers

| Field | Value |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> The parameters below must be sent in JSON format.
    
| Attribute      | Type      | Description                                       | Mandatory | Size         |
|---------------|-----------|----------------------------------------------------|-------------|-----------------|
| http_method   | string    | Method HTTP                                        | yes         | `POST` ou `PUT` |
| url           | url       | URL address that will receive the callback from Webhook | yes         |  255            |
| description   | string    | Webhook description                           | yes         | 255             |
| [**actions**] | **Array** | Thrown Event actions                        | yes         |  -              |

### Ações de Eventos 
> Os valores abaixo devem ser enviados dentro do campo `actions` como texto (string)

|             Evento             |                              Descrição                             |
|:------------------------------:|:------------------------------------------------------------------:|
| `PaymentLinkPaymentSucceeded`  | Payment Link has received a successful payment.                   |
| `PaymentLinkPaymentFailed`     | Payment Link has received a failed payment.                   |
| `SubscriptionPaymentSucceeded` | Subscription payment has been successful.              |
| `SubscriptionPaymentFailed`    | Subscription payment has been failed.                |
| `ChargePaymentSucceeded`       | Charge payment has been successful.                |
| `ChargePaymentFailed`          | Charge payment has been failed.                  |
| `TransactionCreated`           | Transaction has been created.                                            |
| `TransactionWaitingPayment`    | Transaction is Waiting payment.                   |
| `TransactionCanceled`          | Transaction has been Canceled.                             |
| `TransactionPreAuthorized`     | Transaction has been Pre authorized.                        |
| `TransactionCaptured`          | Transaction has been Captured.                             |
| `TransactionDenied`            | Transaction has been Denied.           |
| `TransactionDisputed`          | Transaction has been Disputed.                     |
| `TransactionChargedback`       | Transaction has been reversed, and the amount returned to the Payer / Client. |
| `TransferPaymentSucceeded`     | Transfer's payment has been paid successful.                         |
| `TransferPaymentFailed`        | Transfer's payment has been failed.                        |

### Example of Content to be sent
Check out the example below the content that will be sent on request's body.

```json
{
    "http_method": "POST",
    "url": "https://minhaloja.com.br/callback",
    "description": "Webhook para receber notificações de atualização das transações",
    "actions": [
        "TransactionCreated",
        "TransactionWaitingPayment",
        "TransactionCanceled",
        "TransactionPreAuthorized",
        "TransactionCaptured",
        "TransactionDenied",
        "TransactionDisputed",
        "TransactionChargedback"
    ]
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
  "id": 3,
  "resource": "webhooks",
  "attributes": {
    "http_method": "POST",
    "url": "https://minhaloja.com.br/callback",
    "description": "Webhook para receber notificações de atualização das transações",
    "actions": [
        "TransactionCreated",
        "TransactionWaitingPayment",
        "TransactionCanceled",
        "TransactionPreAuthorized",
        "TransactionCaptured",
        "TransactionDenied",
        "TransactionDisputed",
        "TransactionChargedback"
    ]
  }
}
```

#### In case of failure

```json
{
  "code": "406",
  "message": {
    "url": [
      "Url is required",
      "Url is not a valid URL"
    ]
  },
  "resource": "webhooks.store"
}
```

## Consult Webhook
---
<span class="verb httpGET">GET</span> ***/service/resources/webhooks?id={id}***

---


#### In case of success

```json
{
  "id": 3,
  "resource": "webhooks",
  "attributes": {
    "http_method": "POST",
    "url": "https://minhaloja.com.br/callback",
    "description": "Webhook para receber notificações de atualização das transações",
    "actions": [
        "TransactionCreated",
        "TransactionWaitingPayment",
        "TransactionCanceled",
        "TransactionPreAuthorized",
        "TransactionCaptured",
        "TransactionDenied",
        "TransactionDisputed",
        "TransactionChargedback"
    ]
  }
}
```

## List Webhooks
---
<span class="verb httpGET">GET</span> ***/service/resources/webhooks***

---

#### In case of success

```json
{
  "data": [
    {
      "id": 1,
      "resource": "webhooks",
      "attributes": {
        "http_method": "POST",
        "url": "https://minhaloja.com.br/callback",
        "description": "Webhook para receber notificações de atualização das transações",
        "actions": [
          "ChargePaymentFailed",
          "ChargePaymentSucceeded",
          "SubscriptionPaymentFailed",
          "SubscriptionPaymentSucceeded"
        ]
      }
    },
    {
      "id": 2,
      "resource": "webhooks",
      "attributes": {
        "http_method": "POST",
        "url": "https://minhaloja.com.br/callback",
        "description": "Webhook para receber notificações de atualização dos Links de Pagamento",
        "actions": [
          "PaymentLinkPaymentFailed",
          "PaymentLinkPaymentSucceeded"
        ]
      }
    }
  ],
  "links": {
    "first": "https://api.pluscommercebr.com.br/service/resources/webhooks?page=1",
    "last": "https://api.pluscommercebr.com.br/service/resources/webhooks?page=2",
    "prev": null,
    "next": "https://api.pluscommercebr.com.br/service/resources/webhooks?page=2"
  },
  "meta": {
    "current_page": 1,
    "last_page": 2,
    "from": 1,
    "to": 2,
    "per_page": "2",
    "total": 4
  }
}
```

## Delete Webhook
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/webhooks?id={id}***

---

### Return Code
|   Code  |   Description                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   `404`     |   Resource has been delete previusly or does not exists.                                          |
|   `200`     |   Recurso removido com sucesso. Resource has been deleted                                                                      |
