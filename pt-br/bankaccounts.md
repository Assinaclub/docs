# Contas Bancárias <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por gerenciar Contas Bancárias

## Nova Conta Bancária
---
<span class="verb httpPOST">POST</span> ***/service/resources/bank_accounts***

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|


### Parâmetros
> Os parâmetros abaixo devem ser enviados no BODY em formato JSON

|   Atributo     |   Tipo     |   Descrição               |   Obrigatório                           |   Tamanho  |
|----------------|------------|---------------------------|:-----------------------------------------:|:------------:|
|   code  |   numeric   |   código do Banco |   sim                               |   3       |
|   agency       |   numeric  |   Número da Agência     |   sim  |   4-5       |
|   account   |   numeric  |   Número da Conta Corrente              |   sim      |   20        |
|   type   |   string  |   Tipo da conta              |   não      |   Conta Corrente (Padrão): `checkings` <br>Conta Poupança: `savings`         |
|   external_id   |   string  |   Código Externo              |   não      |   120       |

### Conteúdo de envio
Confira no exemplo abaixo o conteúdo que será enviado no `body` da requisição.

```json
{
	"code": "001",
	"agency": "1234",
	"account": "10023",
    "type": "checkings"
}
```

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.


### Conteúdo de resposta
Confira no exemplo abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
```json
{
  "id": 75,
  "resource": "bank_accounts",
  "attributes": {
    "is_active": true,
    "bank_name": "Banco do Brasil S.A.",
    "bank_code": "001",
    "agency": "1234",
    "number": "10023",
    "type": "checkings",
    "holder_name": "EMPRESA DE TESTES LTDA",
    "cpf_cnpj": "64.518.487/0001-81",
    "created_at": "2021-07-14 09:58:43",
    "updated_at": "2021-07-14 09:58:43"
  }
}
```

#### Em caso de falha:
```json
{
  "code": "406",
  "message": {
    "code": [
      "Code is required",
      "Code must be numeric"
    ],
    "agency": [
      "Agency is required",
      "Agency must be numeric",
      "Agency must not exceed 5 characters"
    ],
    "account": [
      "Account is required",
      "Account must be numeric",
      "Account must not exceed 20 characters"
    ]
  },
  "resource": "bank_accounts.store"
}
```

## Consultar Conta Bancária

> Você pode consultar uma Conta Bancária específica

---
<span class="verb httpGET">GET</span> ***/service/resources/bank_accounts?id={bank_account_id}***

---

### Query Params

|   Atributo     |   Valor                            |   Tipo    |   Obrigatório  |
|----------------|------------------------------------|-----------|----------------|
|   id           |   Identificação da Conta Bancária  |   numeric  |   sim          |


## Listar Contas Bancárias
---

> Você pode listar todas as Contas Bancárias

<span class="verb httpGET">GET</span> ***/service/resources/bank_accounts***

---

## Deletar Conta Bancária

> Você pode deletar uma Conta Bancária específica

---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/bank_accounts?id={bank_account_id}***

---

### Código de Retorno

|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   404     |   Recurso não encontrado                                        |
|   200     |   Recurso removido com sucesso.                                                                       |