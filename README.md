# Easy Pay

**Digital wallet and payments product**: user balances, secure transaction handling and multiple payment providers behind one interface.

> **Showcase repository.** The production source code is private. This repository documents the product, its architecture and the engineering decisions behind it. Details and a walkthrough are available on request: mohammed.r.almekhafi@gmail.com

## Payment flow (simplified)

```mermaid
sequenceDiagram
  autonumber
  participant U as Client app
  participant A as Laravel API
  participant W as Wallet service
  participant P as Payment provider
  participant L as Ledger (DB)
  U->>A: POST /v1/wallet/top-ups (Idempotency-Key)
  A->>W: authorize user · validate limits
  W->>P: create charge
  P-->>A: signed webhook: charge.succeeded
  A->>L: BEGIN · credit balance · audit entry · COMMIT
  A-->>U: 201 Created · updated balance
```

## Engineering highlights

- Provider-agnostic gateway layer: one contract, one driver per payment provider.
- Every balance change runs inside a database transaction and writes an audit entry.
- Provider webhooks are signature-verified and processed asynchronously through queues.
- Role-based access control for customer, merchant and back-office operations.

## Stack

`Laravel` · `PHP` · `MySQL` · `Queues` · `Webhooks` · `RBAC`
