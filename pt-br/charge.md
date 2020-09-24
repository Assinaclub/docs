# Cobrança <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por registrar e consultar Cobranças.

## Nova Cobrança
---
<span class="verb httpPOST">POST</span> ***/service/resources/charges***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

|   Atributo  |   Tipo  |   Descrição  |   Obrigatório  |   Tamanho  |
|-|-|-|-|-|
|   amount  |   numeric  |   Valor da Cobrança  |   sim  |   -  |
|   description  |   string  |   Descrição da Cobrança  |   sim  |   1 - 1000  |
|   due_date  |   date  |   Data de Vencimento da Primeira Parcela da Cobrança  |   sim  |   Y-m-d ou d/m/Y  |
|   frequency  |   integer  |   Frequência da Cobrança (depende do Intervalo)  |   não  |   1 - 12  |
|   interval  |   string  |   Intervalo da Cobrança (diário, semanal, mensal)  |   não  |   `day`, `week` ou `month`  |
|   type  |   string  |   Tipo da Cobrança (Normal ou Recorrente). Padrão: Normal  |   não  |   `normal` ou `recurring`  |
|   last_charge_date  |   date  |   Data da Ultima Cobrança  |   não  |   Y-m-d ou d/m/Y  |
|   callback_url  |   string  |   URL de callback em caso de pagamento  |   não  |   255  |
|   auto_debit  |   boolean  |   Ativar Débito automatizado no Cartão de Crédito. Válido apenas para pagamentos via Cartão de Crédito.  |   não  |   -  |
|   installments  |   integer  |   Parcelamento Prévio da Cobrança. (Simula Carnê)  |   não  |   1 - 48  |
|   is_active  |   boolean  |   Ativar ou Desativar uma Cobrança  |   não  |   -  |
|   **[customer]**  |   **Object**  |   Dados do Cliente  |   sim  |   -  |
|   customer.name  |   string  |   Nome do Cliente  |   sim  |   100  |
|   customer.cpf_cnpj  |   string  |   CPF ou CNPJ do Cliente  |   não  |   CPF ou CNPJ  |
|   customer.phone  |   string  |   Telefone ou Celular do Cliente  |   não  |   10 - 11  |
|   customer.email  |   string  |   Email do Cliente  |   não  |   50  |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

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

#### Em caso de falha

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

## Alterar Cobrança
<span class="verb httpPUT">PUT</span> ***/service/resources/charges?id={id}***

### Parâmetros
> Os parâmetros abaixo podem ser enviados em formato JSON

|   Atributo  |   Tipo  |   Descrição  |   Obrigatório  |   Tamanho  |
|-|-|-|-|-|
|   amount  |   numeric  |   Valor da Cobrança  |   sim  |   -  |
|   description  |   string  |   Descrição da Cobrança  |   sim  |   1 - 1000  |
|   due_date  |   date  |   Data de Vencimento da Primeira Parcela da Cobrança  |   sim  |   Y-m-d ou d/m/Y  |
|   last_charge_date  |   date  |   Data da Ultima Cobrança  |   não  |   Y-m-d ou d/m/Y  |
|   callback_url  |   string  |   URL de callback em caso de pagamento  |   não  |   255  |
|   auto_debit  |   boolean  |   Ativar Débito automatizado no Cartão de Crédito. Válido apenas para pagamentos via Cartão de Crédito.  |   não  |   -  |
|   is_active  |   boolean  |   Ativar ou Desativar uma Cobrança  |   não  |   -  |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "is_active": false
}
```

## Consultar Cobrança
<span class="verb httpGET">GET</span> ***/service/resources/charges?id={id}***

## Listar Cobrança
<span class="verb httpGET">GET</span> ***/service/resources/charges***