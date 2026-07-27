---
name: Top up a Howler cashless tag
description: Authenticate a consumer, locate their RFID cashless tag, start a top-up order, set the amount, and complete payment.
api: openapi/howler-consumer-portal-openapi.yml
operations: [loginUser, getConsumerCashlessTags, updateConsumerCashlessTagTopup, updateConsumerCashlessTagAmounts, getConsumerCashlessTagComplete]
---

# Top up a Howler cashless tag

Howler cashless tags are RFID wearables paired to a consumer at an event. This skill loads the account's tags and adds funds.

## Steps
1. Authenticate the user with `loginUser` (`POST /login`, Basic Authentication) to obtain a JWT bearer token, or use an existing OAuth2 bearer token. Send it as `Authorization: Bearer <token>` (aliases: `x-auth-token` header or `bearer_token` query param).
2. List the user's cashless tags with `getConsumerCashlessTags` (`GET /cashless_tags`) and select the `cashless_tag_uid`.
3. Start a top-up with `updateConsumerCashlessTagTopup` (`POST /cashless_tags/{cashless_tag_uid}/top_up`).
4. Set the amount with `updateConsumerCashlessTagAmounts` (`PUT /cashless_tags/{cashless_tag_uid}/top_up/{top_up_order_id}/amounts`).
5. Confirm completion with `getConsumerCashlessTagComplete` (`GET /cashless_tags/{cashless_tag_uid}/top_up/{top_up_order_id}/completes`).

## Rules
- No Idempotency-Key is supported; do not blind-retry a POST top-up — poll the `pending_successful_payment`/`successful_payment`/`failed_payment` endpoints to resolve state.
- Errors are plain JSON (`{status, message}` / `{errors: {...}}`), not RFC 9457. 401 = re-authenticate; 422 = fix field validation. See errors/howler-problem-types.yml.
