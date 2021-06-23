# Simulation Cards <!-- {docsify-ignore-all} -->

!> The cards entered here are valid but not genuine, and are intended for testing purposes only. For these cards to work as expected you need to have a SIMULATED method configured in your iPag account!

!> Payment Methods must be used ONLY in **Sandbox** environment. This may prevent you from launching your application with an invalid Payment Method.

> If you don't have a Simulated Payment Method configured in your iPag Account yet, <a href="#">Click Here</a>.

## Test Cards

| Bandeira  | Número | Vencimento | CVV | Sempre Aprova |
|-----------|---------|---------|---------|---------|
| `visa` | 4111 1111 1111 1111 | 01/25 | 123 | sim |
| `visa` | 4916 5733 8093 7962 | 12/28 | 123 | não |
| `mastercard` | 5353 2766 5962 9461 | 02/25 | 123 | sim |
| `mastercard` | 5289 0946 0544 4326 | 12/28 | 123 | não |
| `elo` | 4514 1674 2628 1141 | 01/25 | 123 | sim |
| `elo` | 4389 3556 6853 3582 | 12/28 | 123 | não |

### Rules for Card Approval

1. FINAL 1 Cards will always be *approved*.
2. Cards with END DIFFERENT from 1 will be *approved* only if the VALUE of the Transaction's decimal places is EQUAL to 0 (Zero). E.g.: 10.00
3. Cards with END DIFFERENT from 1 will be *rejected* only if the VALUE of the Transaction's decimal places is GREATER than 0 (Zero). E.g.: 10.01 to 10.99