# Estabelecimento {docsify-ignore-all}

## Overview

> Serviço responsável por registrar novos estabelecimentos.

!>	Permitido apenas para contas no Plano Master.

Este serviço realiza o processo de credenciamento de um Estabelecimento Comercial.

## Credenciar Novo Estabelecimento

---
<span class="verb httpPOST">POST</span> https://api.ipag.com.br/service/resources/establishments

---
    
### Headers

| Content-Type  | application/json |
| ------------- | ---------------- | 
| **Authorization** | **Basic** `token` |

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON
    
| Atributo      | Tipo       | Descrição                                                     | Obrigatório | Tamanho                  |
|---------------|------------|---------------------------------------------------------------|-------------|--------------------------|
| name          | string     | Nome da Empresa ou Pessoa Física                              | sim         | 80                       |
| email         | string     | Endereço de e-mail. Será utilizado como login da conta        | sim         | 50                       |
| password      | string     | Senha de acesso que será utilizada no acesso ao Painel        | sim         | 6-20                     |
| document      | string     | Número de CPF ou CNPJ                                         | sim         | -                        |
| phone         | string     | Número de Telefone ou Celular                                 | sim         | -                        |
| **[address]** | **Object** | Dados do Endereço                                             | sim         | -                        |
| street        | string     | Logradouro (Rua, Av, etc...)                                  | sim         | 70                       |
| number        | string     | Número do Endereço                                            | sim         | 10                       |
| district      | string     | Nome do Bairro do Endereço                                    | sim         | 100                      |
| complement    | string     | Complemento do Endereço                                       | sim         | 100                      |
| city          | string     | Nome da Cidade                                                | sim         | 50                       |
| state         | string     | Sigla do Estado                                               | sim         | 2                        |
| zipcode       | string     | CEP do Endereço                                               | sim         | 8                        |
| **[owner]**   | **Object** | **Dados do Responsável/Sócio (Quando Pessoa Jurídica)**       | -           | -                        |
| name          | string     | Nome do Responsável/Sócio da Empresa                          | não         | 80                       |
| email         | string     | Endereço de e-mail do Responsável/Sócio da Empresa            | não         | 50                       |
| cpf           | string     | CPF do Responsável/Sócio da Empresa                           | não         | -                        |
| phone         | string     | Telefone ou Celular do Responsável/Sócio da Empresa           | não         | -                        |
| **[bank]**    | **Object** | **Dados Bancários da Empresa**                                | -           | -                        |
| code          | string     | Código do Banco                                               | não         | 3                        |
| agency        | string     | Número da Agência                                             | não         | 4                        |
| account       | string     | Número da Conta Corrente                                      | não         | 10                       |
| type          | string     | Tipo da Conta                                                 | não         | 'checkings' or 'savings' |
| external_id   | string     | ID Externo da Conta (Exemplo: E-mail registrado no PagSeguro) | não         | 120                      |

### Exemplo de Conteúdo a ser Enviado
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "name": “Academia Interestelar 47",
    "email": "adm@academia.me",
    "password": "123456",
    "document": "72.032.656/0001-05",
    "phone": "(11) 5555-5555",
    "address": {
        "street": “Rua Mariano Lunar",
        "number": "100",
        "district": "Centro",
        "complement": "APTO 412",
        "city": “São Paulo",
        "state": "SP",
        "zipcode": "01156000"
    }
}
```

### Status de Retorno

| Código | Descrição |
| -------| ------ |
| 401 | Não autenticado. Verifique se foi informado corretamente as chaves de API no Basic Authentication.|
| 403 |	Não Autorizado. Sua conta não tem permissão para realizar essa ação. |
| 406 |	Não aceito. Algum dado pode ser inválido verifique os dados enviados no body. |
| 201 |	Recurso criado. |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo         | Tipo       | Descrição                                                            |
|------------------|------------|----------------------------------------------------------------------|
| id               | string     | Identificação do estabelecimento no sistema.                         |
| resource         | string     | Tipo do Recurso                                                      |
| **[attributes]** | **Object** | **Objeto com os atributos do recurso**                               |
| is_active        | boolean    | Informa se o recurso está ativo ou inativo no sistema.               |
| name             | string     | Nome da pessoa física ou Nome Fantasia da Empresa                    |
| business_name    | string     | Nome da Empresa                                                      |
| document         | string     | CPF ou CNPJ                                                          |
| email            | string     | Endereço de e-mail                                                   |
| phone            | string     | Número do telefone ou celular                                        |
| created_at       | date       | Data em que o recurso foi criado                                     |
| updated_at       | date       | Última data que o recurso foi alterado                               |
| **[address]**    | **Object** | **Objeto com os atributos do endereço.**                             |
| street           | string     | Logradouro (Rua, Av, Etc).                                           |
| number           | string     | Número do Endereço                                                   |
| district         | string     | Nome do Bairro do Endereço                                           |
| complement       | string     | Complemento do Endereço                                              |
| city             | string     | Nome da Cidade                                                       |
| state            | string     | Sigla do Estado                                                      |
| zipcode          | string     | CEP do Endereço                                                      |
| **[owner]**      | **Object** | **Objeto com os atributos do sócio responsável em caso de empresa.** |
| name             | string     | Nome do Responsável/Sócio da Empresa                                 |
| cpf              | string     | CPF do Responsável/Sócio da Empresa                                  |
| phone            | string     | Número de telefone do Responsável/Sócio da Empresa                   |
| **[bank]**       | **Object** | **Dados Bancários da Empresa**                                       |
| code             | string     | Código do Banco                                                      |
| agency           | string     | Número da Agência                                                    |
| account          | string     | Número da Conta                                                      |
| type             | string     | Tipo da Conta                                                        |
| external_id      | string     | ID Externo da Conta (Exemplo: E-mail registrado no PagSeguro)        |

### Conteúdo de resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso

```json
{
    "id": "9bd90fed98b9f7f1e9024b13e758c45a",
    "resource": "establishments",
    "attributes": {
    "is_active": true,
    "name": "Academia Interestelar 47",
    "bussiness_name": "Academia Interestelar 47",
    "document": "72.032.656/0001-05",
    "email": "adm@academia.me",
    "phone": “(11) 5555-5555”,
        "address": {
        "street": “Rua Mariano Lunar",
        "number": "100",
        "district": "Centro",
        "complement": "APTO 412",
        "city": “São Paulo",
        "state": "SP",
        "zipcode": "01156000"
    },
    "owner": [],
    "created_at": "2020-01-28",
    "updated_at": “2020-01-28"
    }
}
```

#### Em caso de falha

```json
{
    "code": "406",
    "message": {
        "document": [
            "Document is not a valid CPF\/CNPJ."
        ]
    },
    "resource": "resource.establishments.store"
}
```

## Alterar Estabelecimento
<span class="verb httpPUT">PUT</span> https://api.ipag.com.br/service/resources/establishments?id={id}

Recebe os mesmos atributos do credenciamento, com excessão da senha e e-mail.

## Consultar Estabelecimento
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/establishments?id={id}

## Listar Estabelecimentos
<span class="verb httpGET">GET</span> https://api.ipag.com.br/service/resources/establishments