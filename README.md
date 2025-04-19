
# Stakra - Enhanced Reputation Protocol (ERP)

A Clarity smart contract for managing decentralized participant reputation in a trust-minimized ecosystem. This protocol blends staking, evaluation, and governance into a unified reputation system, ensuring fairness, accountability, and adaptability over time.

---

##  Overview

The **Enhanced Reputation Protocol** is a blockchain-based system that enables:

- Transparent and weighted reputation scoring
- Participant staking and collateral-based accountability
- Authorized evaluator roles with accuracy scoring
- Decay of reputation over time to incentivize continued engagement
- Governance for parameter tuning and protocol evolution

It is designed for **DAOs, decentralized identity systems, contributor networks**, and any system requiring trust-layered participant metrics.

---

##  Features

### Reputation Management
- Reputation scores range from 0 to 100.
- Scores are affected by weighted evaluations based on evaluator accuracy.
- Inactivity triggers **temporal decay** of reputation.

###  Staking and Economic Safeguards
- Participants must lock a minimum amount of STX as **collateral**.
- Collateral is tracked and can be adjusted based on governance rules.

###  Evaluator System
- Evaluators are permissioned and have their own accuracy metrics.
- Evaluations influence participant reputation and are tracked per epoch.

###  Governance Mechanism
- Proposals can be made and voted on.
- Parameter tuning (e.g. epoch length, thresholds) can be governed.
- Voting periods and proposal thresholds are enforced.

###  Evaluation Ledger
- Stores historical evaluation data by epoch.
- Includes metadata and timestamp for auditability.

---

## Deployment & Usage

### Register as Participant

```clarity
(register-participant u1000)
```

- Locks collateral and registers user with maximum reputation.

### Submit Evaluation

```clarity
(submit-participant-evaluation '<participant-principal>' u85 (some "Contributed effectively in Q2"))
```

- Evaluators can submit evaluations that adjust a participant's score based on weighted logic.

### Update Protocol Parameters

```clarity
(update-protocol-parameters {
  min-reputation: u10,
  max-reputation: u100,
  collateral-requirement: u2000,
  epoch-length: u288
})
```

- Only callable by the protocol administrator.
- Must pass validation checks.

### Read Participant Profile

```clarity
(get-participant-profile '<participant-principal>')
```

- Returns decayed reputation, collateral, status, and evaluation count.

---

##  Protocol Constants

| Constant                     | Value     | Description                                  |
|-----------------------------|-----------|----------------------------------------------|
| REPUTATION-MINIMUM          | `0`       | Lowest allowable reputation score            |
| REPUTATION-MAXIMUM          | `100`     | Highest allowable reputation score           |
| MINIMUM-COLLATERAL-REQUIREMENT | `1000` | Minimum STX to stake for registration        |
| COLLATERAL-MULTIPLIER       | `2`       | Used in staking-related calculations         |
| PENALTY-RATE                | `10`      | Penalty deduction rate for violations        |
| EPOCH-LENGTH                | `144`     | Blocks per epoch (~1 day)                    |
| DECAY-INTERVAL              | `10000`   | Interval for reputation decay                |
| DECAY-RATE                  | `5`       | Amount of decay after each interval          |
| PROPOSAL-THRESHOLD          | `75`      | Minimum score to submit governance proposal  |
| VOTING-PERIOD               | `1008`    | Duration for voting (~1 week in blocks)      |

---

##  Data Structures

### `participant-registry`
Stores each participant's:
- Reputation score
- Last activity epoch
- Collateral balance
- Evaluation count
- Status ("ACTIVE", "SUSPENDED", "PROBATION")

### `evaluation-ledger`
Tracks evaluations by:
- Epoch
- Participant
- Score details and evaluator metadata

### `evaluator-credentials`
Tracks:
- Evaluator authorization
- Evaluation accuracy
- Count of evaluations made

### `governance-proposals`
Tracks:
- Proposer, votes, description
- Proposal status: `"ACTIVE"`, `"PASSED"`, `"FAILED"`, `"EXECUTED"`

---

## 🧪 Tests & Audits

Make sure to write unit tests for:

- Edge cases in scoring bounds
- Access control (admin-only functions)
- Evaluator permission logic
- Proposal lifecycle: creation → voting → execution

---

## 🔐 Access Control

| Function                        | Restricted To            |
|--------------------------------|---------------------------|
| `update-protocol-parameters`   | Protocol Administrator    |
| `submit-participant-evaluation`| Authorized Evaluators     |
| `register-participant`         | Any (with sufficient STX) |

---
