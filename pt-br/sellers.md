# Vendedor {docsify-ignore-all}

## Overview

> Serviço responsável por registrar novos vendedores em seu Marketplace.

!> Permitido apenas para contas no Plano Enterprise e Master.

Vendedores podem ser uma Pessoa Física ou Jurídica e são parceiros do Marketplace.

## Registrar Novo Vendedor
---
<span class="verb httpPOST">POST</span> https://api.ipag.com.br/service/resources/sellers

---

### Headers

| Campo | Valor |
| ------------ | ------ |
| Content-Type | application/json |
| Authorization | Basic `Token`|

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON

|   Atributo            |   Tipo    |   Descrição                                                 |   Obrigatório  |   Tamanho  |
|-----------------------|-----------|-------------------------------------------------------------|----------------|------------|
|   login               |   string  |   Login de Acesso a conta Vendedor                          |   sim          |   50       |
|   password            |   string  |   Senha de Acesso a conta Vendedor                          |   sim          |   20       |
|   name                |   string  |   Nome da pessoa física ou Nome Fantasia da Empresa         |   sim          |   80       |
|   cpf_cnpj            |   string  |   CPF ou CNPJ                                               |   sim          |   -        |
|   email               |   string  |   Endereço de email. Será utilizado como login da conta.    |   sim          |   50       |
|   phone               |   string  |   Telefone ou Celular                                       |   sim          |   -        |
|   description         |   string  |   Descrição da Pessoa/Empresa                               |   não          |   255      |
|   **address**         |   **Object**  |   **Dados do Endereço**                                 |   -          |   -        |
|   street              |   string  |   Logradouro (Rua, Av, Etc).                                |   não          |   70       |
|   number              |   string  |   Número do Endereço                                        |   não          |   10       |
|   district            |   string  |   Nome do Bairro do Endereço                                |   não          |   100      |
|   complement          |   string  |   Complemento do Endereço                                   |   não          |   100      |
|   city                |   string  |   Nome da Cidade                                            |   não          |   50       |
|   state               |   string  |   Sigla do Estado                                           |   não          |   2        |
|   zipcode             |   string  |   CEP do Endereço                                           |   não          |   8        |
|   **owner**           |   **Object**  |   **Dados do Responsável/Sócio (Quando Pessoa Jurídica)**  |   -          |   -        |
|   name                |   string  |   Nome do Responsável/Sócio da Empresa                      |   não          |   80       |
|   email               |   string  |   Endereço de e-mail do Responsável/Sócio da Empresa        |   não          |   50       |
|   cpf                 |   string  |   CPF do Responsável/Sócio da Empresa                       |   não          |   -        |
|   phone               |   string  |   Telefone ou Celular do Responsável/Sócio da Empresa       |   não          |   -        |
|   **bank**            |   **Object**  |   **Dados Bancários da Empresa**                                |   -          |   -        |
|   code                |   string  |   Código do Banco                                           |   não          |   3        |
|   agency              |   string  |   Número da Agência                                         |   não          |   4        |
|   account             |   string  |   Número da Conta Corrente                                  |   não          |   10       |

### Conteúdo de envio
Confira no exemplo abaixo o conteúdo que será enviado no `body` da requisição.

```json
{
    "login": "josefrancisco",
    "password": "123123",
    "name":"José Francisco Silva",
    "cpf_cnpj": "854.508.440-42",
    "email": "jose@mail.me",
    "phone": "(11) 98712-1234",
    "description": "",
    "address": {
        "street": "Rua Júlio Gonzalez”,
        "number": "1000",
        "district": "Barra Funda",
        "complement": "10 Andar Sala 3",
        "city": "São Paulo",
        "state": "SP",
        "zipcode": "01156060"
    },
    "owner": {
        "name": "Giosepe",
        "email": "giosepe@teste.com",
        "cpf": "799.993.388-01",
        "phone": "(11) 91333-9876",
        "birthdate": "1988-03-29"
    },
    "bank": {
        "code": "033",
        "agency": "1000",
        "account": "100333"
    }
}
```

### Status de Retorno
|   Código  |   Descrição                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
|   401     |   Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.  |
|   403     |   Não Autorizado. Sua conta não tem permissão para realizar essa ação.                                |
|   406     |   Não aceito. Algum dado pode ser inválido verifique os dados enviados no body.                       |
|   201     |   Recurso criado.                                                                                     |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

|   Atributo      |   Tipo     |   Descrição                                                         |
|-----------------|------------|---------------------------------------------------------------------|
|   id            |   string   |   Identificação do vendedor no sistema.                             |
|   resource      |   string   |   Tipo do Recurso                                                   |
|   [**attributes**]  |   **Object**   |   **Objeto com os atributos do recurso**                              |
|   is_active     |   boolean  |   Informa se o recurso está ativo ou inativo no sistema.            |
|   login         |   string   |   Login de Acesso a conta                                           |
|   name          |   string   |   Nome da pessoa física ou Nome Fantasia da Empresa                 |
|   cpf_cnpj      |   string   |   CPF ou CNPJ                                                       |
|   type          |   string   |   Tipo da Pessoa (Física ou Jurídica)                               |
|   email         |   string   |   Endereço de e-mail                                                |
|   phone         |   string   |   Número do telefone ou celular                                     |
|   description   |   string   |   Descrição do Vendedor                                             |
|   [**address**]     |   **Object**   |   **Objeto com os atributos do endereço.**                              |
|   street        |   string   |   Logradouro (Rua, Av, Etc).                                        |
|   number        |   string   |   Número do Endereço                                                |
|   district      |   string   |   Nome do Bairro do Endereço                                        |
|   complement    |   string   |   Complemento do Endereço                                           |
|   city          |   string   |   Nome da Cidade                                                    |
|   state         |   string   |   Sigla do Estado                                                   |
|   zipcode       |   string   |   CEP do Endereço                                                   |
|   [**owner**]       |   **Object**   |   **Objeto com os atributos do sócio responsável em caso de empresa.**  |
|   name          |   string   |   Nome do Responsável/Sócio da Empresa                              |
|   cpf           |   string   |   CPF do Responsável/Sócio da Empresa                               |
|   phone         |   string   |   Número de telefone do Responsável/Sócio da Empresa                |
|   [**bank**]        |   **Object**   |   **Dados Bancários da Empresa**                                        |
|   code          |   string   |   Código do Banco                                                   |
|   agency        |   string   |   Número da Agência                                                 |
|   account       |   string   |   Número da Conta                                                   |
|   created_at    |   date     |   Data em que o recurso foi criado                                  |
|   updated_at    |   date     |   Última data que o recurso foi alterado                            |

### Conteúdo de resposta
Confira no exemplo abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
```json
{
    "id": "4c63a38de84c8eb2ad7584bbb274af2d",
    "resource": "sellers",
    "attributes": {
        "is_active": true,
        "login": "josefrancisco",
        "name": "José Francisco Silva",
        "cpf_cnpj": "49.703.443/0001-38",
        "type": "Pessoa Jurídica",
        "email": "jose@mail.me",
        "phone": "(11) 98712-1234",
        "description": "",
        "address": {
            "street": "Rua Júlio Gonzalez",
            "number": "1000",
            "district": "Barra Funda",
            "complement": "10 Andar Sala 3",
            "city": "São Paulo",
            "state": "SP",
            "zipcode": "01156060"
        },
        "owner": {
            "name": "Giosepe",
            "cpf": "799.993.388-01",
            "phone": "(11) 91333-9876",
            "birthdate": "1988-03-29"
        },
        "bank": {
            "code": "033",
            "agency": "1000",
            "account": "100333"
        },
        "created_at": "2020-06-17 00:00:00",
        "updated_at": "2020-06-17 00:00:00"
    }
}
```

#### Em caso de falha:
```json
{
  "code": "406",
  "message": {
    "document": [
      "Document is not a valid CPF\/CNPJ."
    ]
  },
  "resource": “resource.sellers.store"
}
```

## Alterar Vendedor
---
<span class="verb httpPUT">PUT</span> https://api.ipag.com.br/service/resources/sellers?id={id}

---

!> Para alterar os dados de um **Vendedor** utilize os mesmos campos descritos no [Registrar Novo Vendedor](pt-br/sellers?id=registrar-novo-vendedor) no `body`.<br>
Neste caso todos os campos são opcionais.

### Conteúdo de envio
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição. Alterando apenas o e-mail de contato do Vendedor:

```json
{
    "email": "contato@mail.me"
}
```

## Consultar Vendedor
---
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/sellers?id={id}

---

## Listar Vendedores
---
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/sellers

---
