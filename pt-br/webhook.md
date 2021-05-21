# Webhook <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por registrar, consultar e excluir Webhooks.

!> Webhook é a forma de recebimento de informações, que são passadas quando um evento acontece.

## Novo Webhook
---
<span class="verb httpPOST">POST</span> ***/service/resources/webhooks***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON
    
| Atributo      | Tipo      | Descrição                                          | Obrigatório | Tamanho         |
|---------------|-----------|----------------------------------------------------|-------------|-----------------|
| http_method   | string    | Método HTTP                                        | sim         | `POST` ou `PUT` |
| url           | url       | Endereço da URL que receberá o callback do Webhook | sim         |  255            |
| description   | string    | Descrição do Webhook                               | sim         | 255             |
| [**actions**] | **Array** | Ações de Eventos disparados                        | sim         |  -              |

### Ações de Eventos 
> Os valores abaixo devem ser enviados dentro do campo `actions` como texto (string)

|             Evento             |                              Descrição                             |
|:------------------------------:|:------------------------------------------------------------------:|
| `PaymentLinkPaymentSucceeded`  | Link de Pagamento recebeu um pagamento aprovado.                   |
| `PaymentLinkPaymentFailed`     | Link de Pagamento recebeu um pagamento recusado.                   |
| `SubscriptionPaymentSucceeded` | O pagamento da Assinatura foi processado com sucesso.              |
| `SubscriptionPaymentFailed`    | O pagamento da Assinatura foi processado com falha.                |
| `ChargePaymentSucceeded`       | O pagamento da Cobrança foi processado com sucesso.                |
| `ChargePaymentFailed`          | O pagamento da Cobrança foi processado com falha.                  |
| `TransactionCreated`           | A Transação foi criada.                                            |
| `TransactionWaitingPayment`    | A Transação está aguardando pagamento do boleto.                   |
| `TransactionCanceled`          | A Transação foi cancelada com sucesso.                             |
| `TransactionPreAuthorized`     | A Transação foi Pré-Autorizada com sucesso.                        |
| `TransactionCaptured`          | A Transação foi Capturada com sucesso.                             |
| `TransactionDenied`            | A Transação teve a autorização recusada pela Adquirente.           |
| `TransactionDisputed`          | A Transação sofreu disputa do Pagador/Cliente.                     |
| `TransactionChargedback`       | A Transação foi Estornada, e o valor devolvido ao Pagador/Cliente. |
| `TransferPaymentSucceeded`     | A Transferência foi realizada com sucesso.                         |
| `TransferPaymentFailed`        | A Transferência foi realizada com falha.                           |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

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

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.    | 


#### Em caso de sucesso

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

#### Em caso de falha

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

## Consultar Webhook
---
<span class="verb httpGET">GET</span> ***/service/resources/webhooks?id={id}***

---


#### Em caso de sucesso

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

## Listar Webhooks
---
<span class="verb httpGET">GET</span> ***/service/resources/webhooks***

---

#### Em caso de sucesso

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
    "first": "https://api.ipag.com.br/service/resources/webhooks?page=1",
    "last": "https://api.ipag.com.br/service/resources/webhooks?page=2",
    "prev": null,
    "next": "https://api.ipag.com.br/service/resources/webhooks?page=2"
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

## Deletar Webhook
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/webhooks?id={id}***

---

### Código de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   `400`     |   Recurso previamente removido ou transação já foi capturada                                          |
|   `200`     |   Recurso removido com sucesso.                                                                       |
