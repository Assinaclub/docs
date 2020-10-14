# Cliente <!-- {docsify-ignore-all} -->

## Overview

> Serviço responsável por gerenciar Clientes.

## Novo Cliente
---
<span class="verb httpPOST">POST</span> ***/service/resources/customers***

---
### Header

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |

### Parâmetros
> Os parâmetros abaixo devem ser enviados em formato JSON

| Atributo | Tipo | Descrição | Obrigatório | Tamanho |
|:-:|:-:|:-:|:-:|:-:|
| id | numeric | id do Cliente | Não | - |
| uuid | string | id único do cliente | Não | - |
| resource | string | Tipo do Recurso | Não | - |
| attributes | Object | Dados do Cliente | Não | - |
| attributes.is_active | boolean | Ativar ou desativar cliente | Sim | - |
| attributes.nome | string | Nome do Cliente | Sim | 100 |
| attributes.cpf_cnpj | string | CPF ou CNPJ do Cliente | Não | `cpf`ou `cnpj` |
| attributes.email | string | Email do Cliente | Não | 80 |
| attributes.phone | string | Telefone do Cliente | Não | 10 ou 11 |
| attributes.address | Object | Dados de endereço do Cliente | Não | - |
| attributes.address.street | string | Rua do endereço do Cliente | Não | 100 |
| attributes.address.number | string | Número do endereço do Cliente | Não | 10 |
| attributes.address.district | string | Nome do bairro ou distrito do Cliente | Não | 80 |
| attributes.address.complement | string | Complemento do endereço do Cliente | Não | 50 |
| attributes.address.city | string | Cidade do Cliente | Não | 80 |
| attributes.address.state | string | Estado do Cliente | Não | 2 |
| attributes.address.zip_code | string | CEP do Cliente | Não | 8 |
| attributes.created_at | date | Data de Cadastro do Cliente | Não | `Y-m-d` ou `d/m/Y` |
| attributes.updated_at | date | Data de última modificação de dados do Cliente | Não | `Y-m-d` ou `d/m/Y` |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "name" : "fulano da silva",
	"cpf_cnpj": "289.610.390-24",
	"email": "fulano@mail.me",
	"phone": "(11) 91234-1200",
	"address": {
		"street": "Rua Marajá",
			"number": "123",
			"district": "Jd. Filadelfia",
			"complement": "",
			"city": "Presidente Prudente",
			"state": "SP",
			"zipcode": "19030-130"
	}
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
  "id": 23809,
  "uuid": "bc59e38bc67f18b4ab36cd450302b8c6",
  "resource": "customers",
  "attributes": {
    "is_active": true,
    "name": "fulano da silva",
    "cpf_cnpj": "289.610.390-24",
    "email": "fulano@mail.me",
    "phone": "(11) 91234-1200",
    "address": {
      "street": "Rua Marajá",
      "number": "123",
      "district": "Jd. Filadelfia",
      "complement": "",
      "city": "Presidente Prudente",
      "state": "SP",
      "zipcode": "19030-130"
    },
    "created_at": "2020-10-08",
    "updated_at": "2020-10-08"
  }
}
```

#### Em caso de falha

```json
{
  "code": "406",
  "message": {
    "address.zip_code": [
      "Address.zip Code is required"
    ]
  },
  "resource": "service.customers.create"
}
```
## Alterar Cliente
<span class="verb httpPUT">PUT</span> ***/service/resources/customers?id={id}***

### Header

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "name" : "Fulano de Oliveira"
}
```

#### Em caso de sucesso

```json
{
  "id": 23809,
  "uuid": "bc59e38bc67f18b4ab36cd450302b8c6",
  "resource": "customers",
  "attributes": {
    "is_active": true,
    "name": "Fulano de Oliveira",
    "cpf_cnpj": "289.610.390-24",
    "email": "fulano@mail.me",
    "phone": "(11) 91234-1200",
    "address": {
      "street": "Rua Marajá",
      "number": "123",
      "district": "Jd. Filadelfia",
      "complement": "",
      "city": "Presidente Prudente",
      "state": "SP",
      "zipcode": "19030-130"
    },
    "created_at": "2020-10-08",
    "updated_at": "2020-10-08"
  }
}
```

## Consultar Cliente
<span class="verb httpGET">GET</span>***/service/resources/customers?id={id}***

## Listar Clientes
<span class="verb httpGET">GET</span>***/service/resources/customers***

## Deletar Cliente
<span class="verb httpDELETE">DELETE</span> ***/service/resources/customers?id={id}***
