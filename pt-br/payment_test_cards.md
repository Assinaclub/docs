# Cartões para Simulação <!-- {docsify-ignore-all} -->

!> Os cartões informados aqui são válidos porém não verdadeiros, e tem o objetivo apenas para testes. Para que esses cartões funcionem da forma esperada é preciso ter um método SIMULADO configurado em sua conta Plus Commerce!

!> Métodos de Pagamento devem ser utilizados SOMENTE em ambiente **Sandbox**. Isso pode evitar que você lance sua aplicação com um Método de Pagamento não válido.

> Caso ainda não possua Método de Pagamento Simulado configurado em sua Conta Plus Commerce, entre em contato com nosso suporte.

## Cartões de Teste

| Bandeira  | Número | Vencimento | CVV | Sempre Aprova |
|-----------|---------|---------|---------|---------|
| `visa` | 4111 1111 1111 1111 | 01/25 | 123 | sim |
| `visa` | 4916 5733 8093 7962 | 12/28 | 123 | não |
| `mastercard` | 5353 2766 5962 9461 | 02/25 | 123 | sim |
| `mastercard` | 5289 0946 0544 4326 | 12/28 | 123 | não |
| `elo` | 4514 1674 2628 1141 | 01/25 | 123 | sim |
| `elo` | 4389 3556 6853 3582 | 12/28 | 123 | não |

### Regras para Aprovação do cartões

1. Cartões com FINAL 1 sempre serão *aprovados*.
2. Cartões com FINAL DIFERENTE de 1 serão *aprovados* somente se o VALOR das casas decimais da Transação for IGUAL a 0 (Zero). Ex.: 10.00
3. Cartões com FINAL DIFERENTE de 1 serão *recusados* somente se o VALOR das casas decimais da Transação for MAIOR que 0 (Zero). Ex.: 10.01 à 10.99
