

# sBTC-Vault - Community Treasury Governance Smart Contract

This Clarity smart contract implements a **decentralized treasury governance system**, enabling a community to:

* Deposit STX funds into a shared treasury
* Propose how funds should be allocated
* Vote on funding proposals
* Automatically execute approved proposals
* Provide emergency shutdown and admin transfer features

## 🛠️ Key Features

* **Treasury Fund Management**: Members can deposit STX into the shared treasury.
* **Proposal Creation**: Propose fund allocation with a description, recipient, and amount.
* **Governance Voting**: One-member-one-vote system for proposal approval.
* **Quorum and Approval Thresholds**: Ensures sufficient participation and consensus.
* **Execution**: Automatic disbursement of funds on proposal approval.
* **Security Measures**: Includes validation, deposit requirements, and admin control.
* **Emergency Shutdown**: Admin can safely drain treasury in emergency situations.

---

## 📘 Smart Contract Functions

### ✅ Public Functions

#### `deposit-funds`

Allows a user to deposit their current STX balance into the treasury.

* Transfers STX from sender to contract
* Increments total treasury balance and user's deposit record

#### `submit-new-proposal (requested-amount uint, funds-recipient principal, proposal-description string)`

Submits a funding proposal.

* Requires a minimum proposal deposit (`1_000_000` µSTX)
* Validates description, amount, and recipient
* Increments total proposal count

#### `cast-vote (proposal-identifier uint, support-proposal bool)`

Casts a vote on an existing proposal.

* Validates that voting is still active
* Prevents duplicate voting

#### `execute-approved-proposal (proposal-identifier uint)`

Executes a proposal if:

* It reached quorum (`>= 500` votes)
* Yes votes meet threshold (`>= 51%`)
* Transfers requested funds to recipient
* Refunds the proposer deposit
* Marks proposal as executed

#### `transfer-admin-rights (new-administrator principal)`

Transfers administrative rights to another principal.

* Only current admin can do this

#### `emergency-treasury-shutdown`

Allows admin to transfer all treasury funds to themselves.

* For use in emergencies only

---

### 🔍 Read-only Functions

* `get-total-treasury-balance`: Returns total treasury STX balance
* `get-proposal-details (proposal-identifier)`: Returns data for a specific proposal
* `check-member-vote-status (proposal-id, member)`: Checks if a member has voted on a proposal
* `get-member-total-deposits (member)`: Shows total deposits by a specific member

---

### 🔒 Internal/Private Functions

* `is-proposal-voting-active`: Checks if voting period is still active
* `check-proposal-quorum-reached`: Evaluates quorum and approval thresholds
* `is-valid-recipient`: Ensures proposal recipient is valid
* `is-valid-description`: Ensures proposal description is not empty or too long

---

## ⚙️ Constants and Thresholds

| Constant                        | Value       | Purpose                                  |
| ------------------------------- | ----------- | ---------------------------------------- |
| `PROPOSAL-VOTING-PERIOD-BLOCKS` | `10_000`    | Length of voting period (in blocks)      |
| `MINIMUM-PROPOSAL-DEPOSIT`      | `1_000_000` | µSTX deposit required to submit proposal |
| `PROPOSAL-QUORUM-THRESHOLD`     | `500`       | Minimum votes required                   |
| `PROPOSAL-APPROVAL-THRESHOLD`   | `510`       | Yes vote threshold (51% of total votes)  |

---

## ❌ Error Codes

| Error Code                        | Meaning                                 |
| --------------------------------- | --------------------------------------- |
| `u100` - Unauthorized access      | Caller is not authorized                |
| `u101` - Insufficient funds       | Treasury cannot cover requested amount  |
| `u102` - Invalid transaction      | Proposal amount must be ≥ 0             |
| `u103` - Proposal not found       | Proposal does not exist                 |
| `u104` - Already voted            | Member already voted on proposal        |
| `u105` - Voting ended             | Voting period is no longer active       |
| `u106` - Proposal deposit too low | Did not meet minimum proposal deposit   |
| `u107` - Invalid recipient        | Cannot send funds to contract or self   |
| `u108` - Invalid description      | Description empty or too long           |
| `u109` - Invalid admin            | Cannot assign admin to self or contract |

---

## 🧠 Suggested Use Cases

* DAO treasury management
* Community grant distribution
* Crowdfunded project governance
* Charity or non-profit fund allocation

---

## 🧾 Deployment Checklist

1. Deploy contract to Clarity-compatible blockchain (e.g. Stacks).
2. Set initial `treasury-administrator`.
3. Fund the contract.
4. Invite users to deposit and submit proposals.

---

## 🔐 Security Considerations

* **Voting Period Enforcement**: Prevents manipulation after expiration.
* **Proposal Deposit Mechanism**: Reduces spam and ensures seriousness.
* **Admin Safeguards**: Emergency and admin functions are restricted.

---
