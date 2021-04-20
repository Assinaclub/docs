# Tokenização de Cartão de Crédito <!-- {docsify-ignore-all} -->

## Novo Token

<span class="verb httpPOST">POST</span> ***/service/resources/card_tokens***

### Headers

| Content-Type | application/json |
| ------------ | ---------------- |
| **Authorization** | **Basic** `token` |

### Parâmetros 
> Os parâmetros abaixo devem ser enviados em formato JSON

#### Cartão de Crédito
| Atributo | Tipo | Descrição | Tamanho |
| -------- | ---- | --------- | ------- |
| **[card]** | **Object** | **Dados do cartão** | - |
| holderName | string | Nome do titular do cartão | 50 |
| number | string | Número do cartão | 19 |
| expiryMonth | string | Mês de vencimento do cartão | 2 |
| expiryYear | string | Ano de vencimento do cartão | 4 |
| cvv | string | Código de segurança do cartão | 4 |

#### Titular do Cartão
| Atributo | Tipo | Descrição | Tamanho |
| -------- | ---- | --------- | ------- |
| **[holder]**| **Object** | **Dados do Titular do Cartão de Crédito** | - |
| name | string | Nome completo do Titular do Cartão | 50 |
| cpfCnpj | string | CPF ou CNPJ do Titular do Cartão | 14 |
| foreignerNumber | string | ID Card ou Número do Passaporte | 30 |
| mobilePhone | string | Número do celular do Titular | 16 |
| birthdate | string | Data de nascimento do titular do Cartão | AAAA-MM-DD |
| **[address]** | **Object** | **Dados do Endereço** | - |
| street | string | Logradouro (Rua, Av, Etc) | 70 |
| number | string | Número do Endereço | 10 |
| district | string | Nome do Bairro do Endereço | 100 |
| complement | string | Complemento do Endereço | 100 |
| city | string | Nome da Cidade | 50 |
| state | string | Sigla do Estado | 2 |
| zipcode |string | CEP do Endereço | 8 |

### Exemplo de Envio
Confira no exemplo abaixo o conteúdo que será enviado no body da requisição.

```json
{
    "card": {
        "holderName": "Frederic Sales",
        "number": "4024 0071 1251 2933",
        "expiryMonth": "02",
        "expiryYear": "2023",
        "cvv": "431"
    },
    "holder": {
        "name": "Frederic Sales",
        "cpfCnpj": "79999338801",
        "mobilePhone": "1899767866",
        "birthdate": "1989-03-28",
        "address": {
            "street": "Rua dos Testes",
            "number": "100",
            "district": "Tamboré",
            "zipcode": "06460080",
            "city": "Barueri",
            "state": "SP"
        }
    }
}
```
    
### Retorno
| Código | Descrição |
| -------- | ---- | 
| 401 | Não autenticado. | Verifique se foi informado corretamente as chaves de API no Basic Authentication. |
| 403 | Não Autorizado. | Sua conta não tem permissão para realizar essa ação. |
| 406 | Não aceito. | Algum dado pode ser inválido verifique os dados enviados no body. |
| 200, 201 | Recurso criado. |

### Estrutura do conteúdo de resposta
Confira no exemplo abaixo a estrutura do conteúdo de resposta desse serviço.

| Atributo | Tipo | Descrição |
| -------- | ---- | --------- | 
| token | string | Token gerado do cartão de crédito  |
| resource | string | Tipo do Recurso: card_token  |
| **[attributes]** | **Object** | **Objeto com os atributos do recurso** |
| validated_at | string | Data em que o token foi validado |
| expires_at | string | Data em que o token irá expirar |
| created_at | date | Data em que o recurso foi criado |
| updated_at | date | Última data que o recurso foi alterado |
| **[card]** | **Object** | **Dados do cartão** |
| is_active | boolean | Informa se o token está ativo para utilização |
| holder | string | Nome do Titular do cartão | 
| bin | string | Seis primeiros dígitos do cartão |
| last4 | string | Últimos quatro dígitos do cartão |
| brand | string | Bandeira do cartão de crédito |
| **holder** | **Object** | **Dados do Titular do cartão** |
| name | string | Nome completo do Titular do cartão |
| cpf | string | CPF/CNPJ do Titular do cartão |
| foreignerNumber | string | ID Card ou Passaporte do Titular do cartão |
| **contacts** | **Object** | **Dados de contato do Titular** |
| type | string | Tipo do contato |
| number | string | Número do contato |
| **addresses** | **Object** | **Coleção de dados dos endereços do Titular** |
| street | string | Logradouro (Rua, Av, Etc) |
| number | string | Número do Endereço |
| district | string | Nome do Bairro do Endereço |
| complement | string | Complemento do Endereço |
| city | string | Nome da Cidade |
| state | string | Sigla do Estado |
| zipcode | string | CEP do Endereço |

### Exemplo de Resposta
Confira nos exemplos abaixo o conteúdo de resposta desse serviço.

#### Em caso de sucesso:
```json
{
    "token": "7be25f8a-ba77-4370-ae19-1e9f57b09797",
    "resource": "card_token",
    "attributes": {
        "card": {
            "is_active": true,
            "holder": "FULANO",
            "bin": "411111",
            "last4": "1111",
            "brand": "visa"
        },
        "holder": {
            "name": "FULANO",
            "cpf": "27151617003",
            "foreignerNumber": "",
            "email": "",
            "contacts": [
                {
                    "type": "mobile",
                    "number": "11997191000"
                }
            ],
            "addresses": [
                {
                    "type": "billing",
                    "street": "Rua João",
                    "number": "1000",
                    "district": "Barra Funda",
                    "complement": "Apto 567",
                    "city": "São Paulo",
                    "state": "SP",
                    "zipcode": "01156060"
                }
            ]
        },
        "validated_at": "2020-06-09 09:42:48",
        "expires_at": "2021-02-28 23:59:59",
        "created_at": "2020-06-09 09:42:48",
        "updated_at": "2020-06-09 09:42:48"
    }
}
```
    
#### Em caso de falha:
```json
{
    "code": "406",
    "message": {
        "holder": [
            "Holder is required",
            "Holder Invalid"
        ],
        "holder.name": [
            "Holder.name is required",
            "Holder.name must be at least 3 characters long",
            "Holder.name must not exceed 80 characters"
        ],
        "holder.cpfCnpj": [
            "Holder.cpfCnpj is required",
            "Holder.cpfCnpj is not a valid CPF/CNPJ."
        ],
        "holder.birthdate": [
            "Holder.birthdate is required",
            "Holder.birthdate must be date with format 'Y-m-d'"
        ],
        "holder.mobilePhone": [
            "Holder.mobilePhone is required",
            "Holder.mobilePhone is not a valid phone."
        ]
    },
    "resource": "resource.card_tokens.store"
}
```
    
## Consultar Token

<span class="verb httpGET">GET</span> ***/service/resources/card_tokens?token={token}***