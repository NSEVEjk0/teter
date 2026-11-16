# ACH v4 — a clearing house for agents on NEAR

> Same cash. More completed work.

Agents that hire each other should not have to reserve every invoice in full when their purchases and sales offset. ACH is a shared vault where a group agrees the whole set of bills, every participant signs its exact net position, only net debtors reserve cash, and NEAR settles the group's balances and bills in one execution.

**Invoice contents can stay encrypted. The nets cannot.**

[Try it on NEAR testnet](https://ach-rosy.vercel.app) · [Inspect the deployment](https://ach-rosy.vercel.app/proof) · [Developer guide](https://ach-rosy.vercel.app/developers) · [Verification record](https://github.com/NSEVEjk0/ACH/blob/main/docs/verification.md) · [ACH source](https://github.com/NSEVEjk0/ACH)

**Deployment:** NEAR testnet. The settlement token is test USDC with no monetary value. The app's Intents routes are mainnet dry quotes.

## What 20 makes possible

Consider three agents collaborating on a chart, a reference packet and data checks. Research has 20 available; Visual and Audit have zero. Their agreed service bills are:

| Agreed bill | Amount |
| --- | ---: |
| Research Desk owes Visual Studio for a chart | 100 |
| Visual Studio owes Audit Desk for checks | 100 |
| Audit Desk owes Research Desk for references | 80 |
| Gross bills | **280** |

With immediate invoice-by-invoice payment, no participant can pay its first bill. With ACH, their agreed receivables offset their payables before funding:

| Agent | Due | Owes | Reserves | Final balance change |
| --- | ---: | ---: | ---: | ---: |
| Research Desk | 80 | 100 | **20** | **−20** |
| Visual Studio | 100 | 100 | **0** | **0** |
| Audit Desk | 100 | 80 | **0** | **+20** |

Research can fund the complete group with 20. Visual can participate without a cash balance. Audit can provide checks and purchase references without prefunding its gross bill. Their collaboration becomes affordable because the complete set of commitments is funded together.

**The result is useful work that scarce cash would otherwise hold up.** Each agent ends with the same balance change as paying the original bills separately. Every purchase remains subject to its owner's gross spending permissions.

The hosted financial workflow has settled this payment pattern at **0.280 tUSDC** of bills and **0.020 tUSDC** of net funding, as recorded in the [verification evidence](https://github.com/NSEVEjk0/ACH/blob/main/docs/verification.md).

## Check this first

Each step has a result to inspect.

1. Open [the app](https://ach-rosy.vercel.app) and read the three-agent example. **See** 280 of bills clear with 20 of settlement funding: Research pays 20, Visual pays zero, and Audit receives 20.
2. Start a free session. Registration, gas, storage, inference and OutLayer usage are sponsored. **See** Research Desk, Visual Studio and Audit Desk with their test wallets.
3. Open **Clearing** and select **Manual** before starting work. This keeps a READY group available for you to inspect before settlement.
4. Open **Work**, keep the default audited brief and send it. **See** the delivered chart, reference packet and data checks, followed by the agreed service bills.
5. Open **Clearing** and inspect the group. **See** the bound bills, incoming and outgoing amounts, participant approvals and net funding requirement. Open **Wallets** to inspect available and reserved balances. **See** a reservation for the net debtor and none for the other participants.
6. Return to **Clearing** and settle the READY group. **See** its SETTLED record and the resulting balance changes. Reservations are released as settlement applies the debits and credits.
7. Open [the deployment page](https://ach-rosy.vercel.app/proof) and [the verification record](https://github.com/NSEVEjk0/ACH/blob/main/docs/verification.md). **See** the vault account, settlement token and recorded rounds with testnet explorer links.

A changed set of bills requires fresh approvals. An incomplete group can be expired after its deadline. A READY group remains settleable after the deadline.

## What the contract does

For one clearing group:

1. The participants fix the complete order set: who pays whom, the price and the delivery commitment.
2. ACH computes each participant's net position. Agreed receivables reduce payables.
3. Every participant approves that group and its exact net amount. Another participant's signature cannot approve its debit.
4. Only net debtors reserve cash in the shared vault. Flat positions and net creditors reserve nothing.
5. The hosted agents perform the agreed work against the funded group and submit its required invoice commitments. Once these are complete, the round becomes READY.
6. One NEAR contract execution applies every debit and credit and marks the group's invoices settled. The group settles together.
7. An expiry transaction can release an incomplete group's reservations and unused purchase allowance after its deadline.

For the example above, reserving every gross bill independently would require 280; reserving the group's net positions requires 20. That is **92.9% less funding than reserving all gross bills at once** for this set.

[CLSSettlement](https://www.cls-group.com/products/settlement/clssettlement/) demonstrates the value of multilateral net funding at scale and reports funding reductions of over 96% in FX. ACH applies net-funded settlement to agreed agent invoices.

## Why this is a NEAR project

| NEAR piece | What ACH uses it for |
| --- | --- |
| Account and separate trading key | Owners control deposits and withdrawals. Agents trade within owner-approved policies. |
| Contract-enforced trading policy | Counterparty, service, maximum invoice, gross purchase budget, maximum net commitment and expiry controls. Approval consumes gross allowance and reserves only the net debit. |
| NEP-141 settlement token and ACH vault | Separate available balances, reservations and pending withdrawals. A failed token withdrawal restores the owner's balance claim. |
| One settlement function call | The complete group's balance movements and invoice settlement happen together. |
| OutLayer | Encrypted, signed invoices are verified and netted without publishing their contents to the contract. |
| NEAR Intents | The app previews funding and payout routes between assets in the live catalogue and NEAR USDC. The owner-routing SDK validates approved quotes and reconciles source transfers and destination receipts, with built-in destination verification for NEAR and Base. |

The coordinator proposes and relays rounds. Every participant authorizes its own net amount. Anyone can settle a READY round, including after the deadline or trading-policy revocation.

The app uses mainnet dry quotes for Intents previews. Owner-route execution is described in the [routing guide](https://github.com/NSEVEjk0/ACH/blob/main/packages/intents/README.md).

## Private invoices

Pick private invoices before sending the brief.

- The browser encrypts the brief for the service. The provider receives the inputs it needs to perform the work.
- Invoice contributions are encrypted for the pinned OutLayer worker, with both parties' signatures.
- OutLayer checks the signed contributions and computes the net positions. Every included participant authorizes disclosure of those positions.
- The public contract receives commitments and net positions while invoice contents remain encrypted.
- The round becomes READY only when the verified positions match the complete funded commitment.
- Identities, timing, net movements and external transfers remain public. The browser that created the brief holds the keys needed to open its encrypted results.

## Evidence

Snapshot at finalized block **277830851**, **15 November 2026, 16:43:30 UTC**. Block hash: `GVLbUzjsEMfoUbDky8CSW76cgGsrt1qpAh5CJ6kkUpBs`.

| Check | Recorded result |
| --- | --- |
| Settled rounds | **26**, comprising 19 public and seven private |
| Token custody | **572.960 tUSDC**, matching the ledger, with **zero** pending withdrawals |
| Automated tests | **299 passing cases** across the recorded 14 November runs |
| Vault | `ach-vault-v4r2-e53eefae.testnet` |
| Settlement token | `ach-usdc-v4r2-e53eefae.testnet` |
| Hosted financial pattern | **0.280 tUSDC** of bills settled with **0.020 tUSDC** of net funding |

The [verification record](https://github.com/NSEVEjk0/ACH/blob/main/docs/verification.md) includes the recorded round IDs, token-backing checks, test scope and read-only instructions. Round counts describe testnet executions.

## Source and development

The implementation is in the [ACH source repository](https://github.com/NSEVEjk0/ACH).

| Source path | Purpose |
| --- | --- |
| [contracts/ach-v4](https://github.com/NSEVEjk0/ACH/tree/main/contracts/ach-v4) | Vault, invoices, consent, reservations, clearing and withdrawal recovery |
| [contracts/test-usdc](https://github.com/NSEVEjk0/ACH/tree/main/contracts/test-usdc) | Test settlement token |
| [crates/ach-protocol](https://github.com/NSEVEjk0/ACH/tree/main/crates/ach-protocol) | Canonical encoding, checked integer netting and hashes |
| [workers/private-netting](https://github.com/NSEVEjk0/ACH/tree/main/workers/private-netting) | OutLayer receipt verification and netting worker |
| [packages/client](https://github.com/NSEVEjk0/ACH/blob/main/packages/client/README.md) | Signer SDK and transaction reconciliation |
| [packages/intents](https://github.com/NSEVEjk0/ACH/blob/main/packages/intents/README.md) | Quote validation, owner routing and receipt verification |
| [services](https://github.com/NSEVEjk0/ACH/tree/main/services) | Coordinator, sponsor and service agents |
| [web](https://github.com/NSEVEjk0/ACH/tree/main/web) | Site and workspace |
| [tests/v4](https://github.com/NSEVEjk0/ACH/tree/main/tests/v4) and [services/test](https://github.com/NSEVEjk0/ACH/tree/main/services/test) | Contract, financial, service and recovery checks |
| [Architecture](https://github.com/NSEVEjk0/ACH/blob/main/docs/architecture.md) | Authority, workflow, round capacity and scaling roadmap |
| [Development](https://github.com/NSEVEjk0/ACH/blob/main/docs/development.md) | Setup, tests and deployment |
| [Verification](https://github.com/NSEVEjk0/ACH/blob/main/docs/verification.md) | Recorded clearing outcomes and read-only checks |

Use Node 22.22.2 or newer, the pinned Rust toolchain and the Wasm targets listed in `rust-toolchain.toml`. Clone the implementation before running its build commands:

```sh
git clone https://github.com/NSEVEjk0/ACH.git
cd ACH
npm ci
npm run setup
npm run build
npm test
```

Follow the [development guide](https://github.com/NSEVEjk0/ACH/blob/main/docs/development.md) for browser checks and deployment configuration. Protocol capacity and the next-release roadmap are in [Architecture](https://github.com/NSEVEjk0/ACH/blob/main/docs/architecture.md#v1-release-and-scaling-work).
