# Assinatura <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por gerenciar Assinaturas.

## Nova Assinatura
---
<span class="verb httpPOST">POST</span> ***/service/resources/subscriptions***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Accept | application/json |

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

| Atributo | Tipo | Descrição | Obrigatório | Tamanho |
|-|-|-|:-:|:-:|
| is_active | boolean | Define se a Assinatura estará ativa ou inativa | não | `true` ou `false` |
| profile_id | string | Campo reservado para identificação da Assinatura definida pelo Integrador | sim | 100 |
| plan_id | integer | Identificação única do Plano de Assinatura | sim | - |
| customer_id | integer | Identificação única do Cliente | sim | - |
| starting_date | date | Data do primeiro vencimento (inicio) da cobrança da Assinatura | sim | `AAAA-mm-dd` |
| closing_date | date | Data de fechamento da Assinatura | não | `AAAA-mm-dd` |
| callback_url | url | URL de Callback para notificação quando a Assinatura for paga | sim | 255 |
| creditcard_token | string | Token único do Cartão Tokenizado | não | 36 |


### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
	"is_active": true,
	"profile_id": "SUB_001",
	"plan_id": 453,
	"customer_id": 23817,
	"starting_date": "2021-07-10",
	"callback_url": "https://minhaloja.com/callback"
}
```
### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.    |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo | Tipo | Descrição |
|-|:-:|-|
| type | string | Tipo do Recurso |
| id | numeric | ID da Assinatura |
| [**attributes**]▼ | **Object** | Dados da Assinatura |
| is_active | boolean | Define se a Assinatura está ativa ou inativa |
| status | string | Define a situação da Assinatura: `pending`, `paid`, `overdue`. Pendente, Paga, Atrasada, respectivamente. |
| description | string | Descrição da Assinatura. Atribuído pelo Plano de Assinatura |
| profile_id | string | Código de Perfil para identificação da Assinatura, definida pelo integrador. |
| amount | numeric | Valor da Assinatura. Atribuído pelo Plano de Assinatura |
| installments | numeric | Quantidade de Parcelas que será aplicado na cobrança da Assinatura. Padrão: 1 |
| frequency | numeric | Frequência com qual o Intervalo será definido. |
| interval | string | Intervalo: `month`, `week`, `day` (Mensal, Semanal ou Diário) |
| charging_day | string\|numeric | Dia da cobrança. Definido pela Data de Início . |
| starting_date | date | Data do primeiro vencimento (inicio) da cobrança da Assinatura |
| closing_date | date | Data de fechamento da Assinatura |
| callback_url | url | URL de Callback para notificação quando a Assinatura for paga |
| created_at | date | Data da criação da Assinatura |
| updated_at | date | Data da última alteração realizada na Assinatura |
| plan | **Object** | Dados do Plano de Assinatura |
| creditcard | **Object** | Dados do Cartão Tokenizado |
| customer | **Object** | Dados do Cliente |
| invoices | **List** | Lista das Faturas que já foram registradas. |
| links.payment | url | Link do Checkout para Pagamento, pode ser enviado ao Cliente. |


### Em caso de sucesso

```json
{
  "id": 1799,
  "resource": "subscriptions",
  "attributes": {
    "is_active": true,
    "status": "pending",
    "description": "Plano Básico de Assinatura",
    "profile_id": "SUBS_002",
    "amount": 59,
    "installments": 1,
    "frequency": 1,
    "interval": "month",
    "charging_day": "10",
    "starting_date": "2021-07-10",
    "closing_date": "0000-00-00",
    "callback_url": "https://minhaloja.com/callback",
    "created_at": "2021-07-02 10:16:52",
    "updated_at": "2021-07-02 10:16:52",
    "plan": {
      "type": "plans",
      "id": 453,
      "attributes": {
        "name": "Plano Start",
        "description": "Plano Básico de Assinatura",
        "amount": 59,
        "frequency": "monthly",
        "interval": 1,
        "cycles": 0,
        "best_day": false,
        "pro_rated_charge": false,
        "grace_period": 0,
        "trial": {
          "amount": 0,
          "cycles": 0
        },
        "links": {
          "payment": "https://api.ipag.com.br/subscriptions?id=49no49a23f67c759bf4fc791ba842aa2d836cfa71fa88dffa23d2299247d651f68b6d410"
        },
        "created_at": "2018-03-15 10:02:54",
        "updated_at": "2018-03-15 10:02:54"
      }
    },
    "creditcard": null,
    "customer": {
      "id": 23817,
      "uuid": "8f20fe72886d5849926d898864c46523",
      "resource": "customers",
      "attributes": {
        "is_active": true,
        "name": "FULANO DA SILVA",
        "business_name": "FULANO DA SILVA",
        "cpf_cnpj": "73396153006",
        "email": "fulano@mail.me",
        "phone": "1832031008",
        "address": {
          "street": "RUA TESTE",
          "number": "16",
          "district": "",
          "complement": "",
          "city": "SAO PAULO",
          "state": "SP",
          "zipcode": "01156060"
        },
        "created_at": "2020-11-09",
        "updated_at": "2020-11-09"
      }
    },
    "invoices": [
      {
        "number": 1,
        "due_date": "2021-07-10",
        "amount": 59,
        "paid_amount": 0,
        "discount": 0,
        "payment_date": null,
        "description": "Plano Básico de Assinatura",
        "payment": null
      }
    ],
    "links": {
      "payment": "https://api.ipag.com.br/vpos?billing=1833a888904bd4867929dffd884d60b8"
    }
  }
}
```

## Alterar Assinatura
---
<span class="verb httpPUT">PUT</span> ***/service/resources/subscriptions?id={id}***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

| Atributo | Tipo | Descrição | Obrigatório | Tamanho |
|-|-|-|:-:|:-:|
| is_active | boolean | Define se a Assinatura estará ativa ou inativa | não | `true` ou `false` |
| profile_id | string | Campo reservado para identificação da Assinatura definida pelo Integrador | não | 100 |
| plan_id | integer | Identificação única do Plano de Assinatura | não | - |
| customer_id | integer | Identificação única do cliente | não | - |
| starting_date | date | Data do primeiro vencimento (inicio) da cobrança da Assinatura | não | `AAAA-mm-dd` |
| closing_date | date | Data de fechamento da Assinatura | não | `AAAA-mm-dd` |
| callback_url | url | URL de Callback para notificação quando a Assinatura for paga | não | 255 |
| creditcard_token | string | Token único do Cartão Tokenizado | não | 36 |



### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que poderá ser enviado no body da requisição.

#### Exemplo de como alterar o Token do Cartão de Crédito
```json
{
    "creditcard_token" : "1dfd4db8-2e79-402f-9720-a943f5143e71"
}
```

!> É possível utilizar esta chamada para alterar o cartão de crédito de um Cobrança Recorrente também.

#### Exemplo de como desativar a Assinatura
```json
{
    "is_active" : false
}
```


## Consultar Assinatura
---
<span class="verb httpGET">GET</span> ***/service/resources/subscriptions?id={id}***

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id        |   string |   ID Único da Assinatura |


> [Retorno da Consulta](pt-br/subscription?id=em-caso-de-sucesso)

## Listar Assinaturas
---
<span class="verb httpGET">GET</span> ***/service/resources/subscriptions***

---

### Query Params (Filtros)

| Atributo | Tipo | Descrição |
|-|:-:|-|
| is_active | boolean | Assinatura Ativa ou Inativa |
| from_date | date | Filtro de Data começando em |
| to_date | date | Filtro de Data acabando em |
| status | string | `pending`, `paid`, `overdue` |
| customer_name | string | Nome do cliente |
| limit | integer | Quantidade de Assinaturas por Página. Padrão 15 |
| page | integer | Número da Página |

!> Para utilizar o filtro de Período `from_date` e `to_date` devem ser informados.

-

## Quitar Parcela da Assinatura

---
<span class="verb httpPOST">POST</span> ***/service/resources/invoice_installments?subscription_id={`subscription_id`}&invoice_number={`invoice_number`}&action=`pay`***

---

### Query Params 

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   subscription_id       |   integer |   ID Único da Assinatura |
|   invoice_number       |   integer |   Número da Parcela (Fatura) |
|   action       |   string |   Ação ser realizada, utilizar `pay` |


### Exemplo de Envio

`POST` https://api.ipag.com.br/service/resources/invoice_installments?subscription_id=1725&invoice_number=3&action=pay

### Em caso de sucesso

```json
{
  "subscription_id": 1725,
  "number": 3,
  "due_date": "2021-01-20",
  "amount": 15,
  "paid_amount": 15,
  "discount": 0,
  "payment_date": "2021-07-08",
  "description": "Assinatura",
  "payment": null
}
```

### Em caso de falha

```json
{
  "code": "400",
  "message": "Invoice Installment #  has been paid already",
  "resource": "resource.subscription_invoice"
}
```

## Agendar Pagamento de Parcela da Assinatura

!> Somente é possível agendar pagamentos de parcelas com vencimento para hoje ou data inferior (vencida).

!> Somente é possível agendar pagamentos que ainda não foram quitados, ou seja, que possuem valor de pagamento zerado.

---
<span class="verb httpPOST">POST</span> ***/service/resources/invoice_installments?subscription_id={`subscription_id`}&invoice_number={`invoice_number`}&action=`schedule`***

---

### Query Params 

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   subscription_id       |   integer |   ID Único da Assinatura |
|   invoice_number       |   integer |   Número da Parcela (Fatura) |
|   action       |   string |   Ação ser realizada, utilizar `schedule` |


### Exemplo de Envio

`POST` https://api.ipag.com.br/service/resources/invoice_installments?subscription_id=1725&invoice_number=3&action=schedule

### Em caso de sucesso

HTTP `201 Created`

```json
'ok'
```

### Em caso de falha

```json
{
  "code": "400",
  "message": "Installment #3 of Invoice #1725 has been queued already",
  "resource": "resource.subscription_invoice"
}
```

ou 

```json
{
  "code": "400",
  "message": "Invoice Installment #2 has been paid already, it can't be scheduled.",
  "resource": "resource.subscription_invoice"
}
```