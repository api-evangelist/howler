---
name: Assign or transfer a Howler ticket
description: List a consumer's tickets, then assign one to a named attendee or transfer it to another user.
api: openapi/howler-consumer-portal-openapi.yml
operations: [getConsumerTickets, updateConsumerTicketAssign, updateConsumerTicketTransferCreate, updateConsumerTicketTransferConfirm]
---

# Assign or transfer a Howler ticket

## Steps
1. Authenticate and send the bearer token (see conventions/howler-conventions.yml).
2. List tickets with `getConsumerTickets` (`GET /tickets`) and pick a `ticket_id`.
3. To assign a ticket to a named attendee, call `updateConsumerTicketAssign` (`PUT /tickets/{ticket_id}/assign`).
4. To transfer ownership to another user, start with `updateConsumerTicketTransferCreate` (`POST /tickets/{ticket_id}/transfer`), then confirm with `updateConsumerTicketTransferConfirm` (`PUT /tickets/{ticket_id}/transfer/{transfer_order_id}/confirmation`).

## Rules
- Ticket `status` enum includes pending, active, transferred, refunded, on_resale, sold — check status before assigning/transferring.
- Handle 403 (not the ticket owner) and 422 (validation) per errors/howler-problem-types.yml.
