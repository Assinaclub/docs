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
| nome | string | Nome do Cliente | Sim | 100 |
| cpf_cnpj | string | CPF ou CNPJ do Cliente | Não | `cpf`ou `cnpj` |
| email | string | Email do Cliente | Não | 80 |
| phone | string | Telefone do Cliente | Não | 10 ou 11 |
| [**address**]&#9660; | **Object** | Dados de endereço do Cliente | Não | - |
| address.street | string | Rua do endereço do Cliente | Não | 100 |
| address.number | string | Número do endereço do Cliente | Não | 10 |
| address.district | string | Nome do bairro ou distrito do Cliente | Não | 80 |
| address.complement | string | Complemento do endereço do Cliente | Não | 50 |
| address.city | string | Cidade do Cliente | Não | 80 |
| address.state | string | Estado do Cliente | Não | 2 |
| address.zipcode | string | CEP do Cliente | Não | 8 |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
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

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo | Tipo | Descrição |
|:-:|:-:|:-:|
| id | numeric | ID do Cliente |
| uuid | string | Identificação única do cliente |
| resource | string | Tipo do Recurso |
| [**attributes**]&#9660; | **Object** | Dados do Cliente |
| is_active | boolean | Ativar ou desativar cliente |
| nome | string | Nome do Cliente |
| cpf_cnpj | string | CPF ou CNPJ do Cliente |
| email | string | Email do Cliente |
| phone | string | Telefone do Cliente |
| created_at | date | Data de Cadastro do Cliente |
| updated_at | date | Data de última modificação de dados do Cliente |
| [**address**]&#9660; | **Object** | Dados de endereço do Cliente |
| address.street | string | Rua do endereço do Cliente |
| address.number | string | Número do endereço do Cliente |
| address.district | string | Nome do bairro ou distrito do Cliente |
| address.complement | string | Complemento do endereço do Cliente |
| address.city | string | Cidade do Cliente |
| address.state | string | Estado do Cliente |
| address.zipcode | string | CEP do Cliente |

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
    "address.zipcode": [
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

### Em caso de sucesso

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
---
<span class="verb httpGET">GET</span> ***/service/resources/customers?id={id}***

---

> [Retorno da Consulta](pt-br/customer?id=em-caso-de-sucesso)

## Listar Clientes
---
<span class="verb httpGET">GET</span> ***/service/resources/customers***

---

### Query Params (Filters)

| Atributo  | Tipo    | Descrição                            |
|-----------|---------|--------------------------------------|
| name      | string  | Nome do Cliente                      |
| cpf_cnpj  | string  | CPF ou CNPJ do Cliente               |
| email     | string  | E-mail do Cliente                    |
| phone     | string  | Telefone do Cliente                  |
| is_active | boolean | Status do Cliente (`true` ou `false`) |
| order      | string | Ordernação dos Resultados (`asc` ou `desc`) |

## Deletar Cliente
---
<span class="verb httpDELETE">DELETE</span> ***/service/resources/customers?id={id}***

---

> Em caso de sucesso o **Response** será `200 OK`

!> Um Cliente só poderá ser deletado se, e somente se não houver nenhum Recurso utilizando-o.
