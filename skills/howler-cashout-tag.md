---
name: Cash out a Howler cashless tag balance
description: Refund the remaining balance on a consumer's cashless tag to their bank account after an event.
api: openapi/howler-consumer-portal-openapi.yml
operations: [showConsumerBankAccount, createConsumerBankAccount, updateConsumerCashoutEvent, updateConsumerCashoutAmounts, updateConsumerCashoutConfirm]
---

# Cash out a Howler cashless tag balance

## Steps
1. Authenticate and send the bearer token.
2. Ensure a bank account exists: `showConsumerBankAccount` (`GET /bank_account`); if absent, `createConsumerBankAccount` (`POST /bank_account`).
3. Start the cashout with `updateConsumerCashoutEvent` (`POST /cashless_tags/{cashless_tag_uid}/cashout`).
4. Set the amount with `updateConsumerCashoutAmounts` (`PUT /cashless_tags/{cashless_tag_uid}/cashout/{cashless_tag_pairing_id}/amounts`).
5. Confirm with `updateConsumerCashoutConfirm` (`PUT /cashless_tags/{cashless_tag_uid}/cashout/{cashless_tag_pairing_id}/confirmation`).

## Rules
- Cashout moves real money; treat POST/PUT as non-idempotent and confirm state before retrying.
- A valid bank account is required or the cashout will fail with 422.
