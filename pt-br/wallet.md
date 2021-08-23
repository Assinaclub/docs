# Carteira Digital <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por visualizar dados da Carteira Digital

!> Este recurso está disponível somente para Estabelecimento que possuem Subadquirência nativa.

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

## Visualizar Saldo da Carteira
---
<span class="verb httpGET">GET</span> ***/service/account/funds_balance***

---

### Conteúdo de resposta
Confira no exemplo abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
```json
{
  "account_balance": 189.23
}
```

## Solicitar Saque do Saldo da Carteira

!> Somente é possível solicitar o Saldo Total disponível na Carteira

!> Pode haver insidência de taxas para o saque!

---
<span class="verb httpPOST">POST</span> ***/service/account/funds_balance***

---

### Conteúdo de resposta
Confira no exemplo abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
```json
{
  "transfer": {
    "id": 17,
    "resource": "transfers",
    "attributes": {
      "transfer_number": "393ed9fdf1f0dbb00d5624cb99e4e5f6",
      "transfer_source": "Conta Corrente",
      "status": "new",
      "amount": 100.95,
      "original_amount": 100.94,
      "currency": "BRL",
      "statement_descriptor": "MANUAL TRANSFER",
      "description": "",
      "created_at": "2021-07-14 10:48:54",
      "updated_at": "2021-07-14 10:48:54",
      "seller": {},
      "bank_account": {}
    }
  },
  "message": "Transfer Request Succeeded"
}
```

#### Em caso de falha:
```json
{
  "code": "400",
  "message": "Insuficient funds",
  "resource": "transfers.request"
}
```
