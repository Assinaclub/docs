# Transferência <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por listar e consultar transferências.

## Lista Transferências
---
<span class="verb httpGET">GET</span> ***/service/resources/transfers***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.                                                                                     |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo            |   Tipo     |   Descrição                                   |
|-----------------------|------------|-----------------------------------------------|
|   **data**                |   **Array**    |   **Lista de Transferências**                     |
|   id                  |   string   |   Identificação da transferência no sistema.  |
|   resource            |   string   |   Tipo do Recurso                             |
|   **[attributes]**        |   **Object**   |   **Objeto com os atributos do recurso**          |
|   transfer_number     |   boolean  |   Código identificador da transferência       |
|   transfer_source     |   string   |   Fonte para onde foi transferido o recurso   |
|   status              |   string   |   Status da Transferência                     |
|   amount              |   string   |   Valor Liquido                               |
|   original_amount     |   string   |   Valor Original                              |
|   currency            |   string   |   Moeda                                       |
|   description         |   string   |   Descrição               |
|   created_at          |   string   |   Data de criação da transferência            |
|   updated_at          |   string   |   Data de atualização da transferência        |

### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

Em caso de sucesso:
```json
{
    "data": [
        {
            "id": 4,
            "resource": "transfers",
            "attributes": {
                "transfer_number": "5ab6987a6ee1970dacadc7fc1909dd8c",
                "transfer_source": “bank account”,
		    "receiver_id": “308a4f1079d3c50653dfeca80d85ea6a",
                "status": "failed",
                "amount": 25.48,
                "original_amount": 25.48,
                "currency": “BRL",
                "description": "",
                "created_at": "2018-12-20 16:54:44",
                "updated_at": "2018-12-20 16:54:44"
            }
        }
    ],
    "links": {
        "first": “https://api.ipag.com.br/service/resources/transfers?page=1”,
        "last": “https://api.ipag.com.brservice/resources/transfers?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "last_page": 1,
        "from": 1,
        "to": 3,
        "per_page": 15,
        "total": 3
    }
}
```
### Caso de nenhuma transferência encontrada:

```json
{
    "data": []
}
```

## Consultar Transferência

---
<span class="verb httpGET">GET</span> ***/service/resources/transfers?id={id}***

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id      |   string  |   Identificação única da Transferência  |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo            |   Tipo     |   Descrição                                         |
|-----------------------|------------|-----------------------------------------------------|
|   id                  |   string   |   Identificação da transferência no sistema.        |
|   resource            |   string   |   Tipo do Recurso                                   |
|   **[attributes]**        |   **Object**   |   **Objeto com os atributos do recurso**                |
|   transfer_number     |  string  |   Código identificador da transferência             |
|   transfer_source     |   string   |   Fonte para onde foi transferido o recurso         |
|   status              |   string   |   Status da Transferência                           |
|   amount              |   string   |   Valor Liquido                                     |
|   original_amount     |   string   |   Valor Original                                    |
|   currency            |   string   |   Moeda                                             |
|   description         |   string   |   Descrição                    |
|   created_at          |   string   |   Data de criação da transferência                  |
|   updated_at          |   string   |   Data de atualização da transferência              |
|   **receivables**         |   **Object**   |   **Objeto com recebíveis vinculados a transferência**  |

### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

Em caso de sucesso:

```json
{
    "id": 4,
    "resource": "transfers",
    "attributes": {
        "transfer_number": "5ab6987a6ee1970dacadc7fc1909dd8c",
        "transfer_source": “bank account",
        "receiver_id": "308a4f1079d3c50653dfeca80d85ea6a",
        "status": "failed",
        "amount": 25.48,
        "original_amount": 25.48,
        "currency": “BRL",
        "description": "",
        "created_at": "2018-12-20 16:54:44",
        "updated_at": "2018-12-20 16:54:44”,
        "receivables": {
            "data": [
                {
                    "id": 8,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005534",
                        "status": "pending",
                        "amount": 9.2,
                        "gross_amount": 10,
                        "installment": 1,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-11-20 15:28:02",
                        "created_at": "2018-11-19 15:28:02",
                        "updated_at": "2018-11-19 15:28:02"
                    }
                },
                {
                    "id": 9,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005877",
                        "status": "pending",
                        "amount": 8.14,
                        "gross_amount": 9,
                        "installment": 1,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-12-07 10:54:40",
                        "created_at": "2018-12-06 10:54:40",
                        "updated_at": "2018-12-06 10:54:40"
                    }
                },
                {
                    "id": 10,
                    "resource": "receivables",
                    "attributes": {
                        "transaction": "1005877",
                        "status": "pending",
                        "amount": 8.14,
                        "gross_amount": 9,
                        "installment": 2,
                        "description": "",
                        "paid_at": "",
                        "canceled_at": "",
                        "expected_on": "2018-12-07 10:54:40",
                        "created_at": "2018-12-06 10:54:40",
                        "updated_at": "2018-12-06 10:54:40"
                    }
                }
            ]
        }
    }
}

```

Em caso de transferência não encontrada:

```json
{
    "code": "404",
    "message": "Transfer Not Found",
    "resource": "resource.transfers.view"
}
```

## Listar Transferência dos Vendedores

---
<span class="verb httpGET">GET</span> ***/service/resources/sellers_transfers***

---

### Estrutura do conteúdo de resposta
A estrutura segue o mesmo padrão do [Listar Transferências](pt-br/transfers?id=listar-transferências).

## Consultar Transferência de Vendedor
---
<span class="verb httpGET">GET</span> ***/service/resources/sellers_transfers?id={id}***

---

### Query Params

|   Atributo  |   Tipo   |   Descrição                |
|-------------|----------|----------------------------|
|   id      |   string  |   Identificação única da Transferência  |

### Estrutura do conteúdo de resposta
A estrutura segue o mesmo padrão do [Consultar Transferência](pt-br/transfers?id=consultar-transferência).
