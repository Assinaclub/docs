# Transferência Futura <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por listar os recebíveis futuros.

## Listar Lançamentos Futuros
---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo            |   Tipo     |   Descrição                                   |
|-----------------------|------------|-----------------------------------------------|
|   **data**                |   **Array**    |   **Lista de Ordenada por Data dos Recebíveis**         |
|   *data*                  |   date   |   Data esperada do pagamento.                                 |
|   sub_total            |   number   |   Total esperado para pagamento na data especificada.          |
|   **[receivables]**        |   **Array**   |   **Objeto com os atributos do recurso (receivables)**  |
|   expected_total_amount        |   number   |   Soma Total do esperado para todos os pagamentos futuros.          |

### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

Em caso de sucesso:
```json
{
  "data": {
    "2020-09-04": {
      "sub_total": 3.13,
      "receivables": [
        {
          "id": 917,
          "resource": "receivables",
          "attributes": {
            "receiver_id": "4c63a38de84c8eb2ad7584bbb274af2d",
            "transaction": "1011826",
            "status": "pending",
            "amount": 1.59,
            "gross_amount": 1.79,
            "installment": 9,
            "description": "",
            "paid_at": "",
            "canceled_at": "",
            "expected_on": "2020-09-04 10:00:00",
            "created_at": "2020-07-03 17:15:20",
            "updated_at": "2020-07-03 17:15:20"
          }
        },
        {
          "id": 918,
          "resource": "receivables",
          "attributes": {
            "receiver_id": "4c63a38de84c8eb2ad7584bbb274af2d",
            "transaction": "1011826",
            "status": "pending",
            "amount": 1.54,
            "gross_amount": 1.78,
            "installment": 10,
            "description": "",
            "paid_at": "",
            "canceled_at": "",
            "expected_on": "2020-09-04 10:00:00",
            "created_at": "2020-07-03 17:15:20",
            "updated_at": "2020-07-03 17:15:20"
          }
        }
      ]
    }
  },
  "links": {
    "first": "https://api.pluscommercebr.com.br/service/resources/future_transfers?page=1",
    "last": "https://api.pluscommercebr.com.br/service/resources/future_transfers?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "last_page": 1,
    "from": 1,
    "to": 1,
    "per_page": 30,
    "total": 1
  },
  "expected_total_amount": 3.13
}
```

### Caso não exista recebíveis futuros:

```json
{
    "data": []
}
```

## Listar Lançamentos Futuros de Vendedor
---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers?seller_id={id}***

---

Ou

---
<span class="verb httpGET">GET</span> ***/service/resources/future_transfers?cpf_cnpj={cpf_cnpj}***

---

### Estrutura do conteúdo de resposta
A estrutura segue o mesmo padrão do [Listar Lançamentos Futuros](pt-br/future_transfers?id=listar-lançamentos-futuros).