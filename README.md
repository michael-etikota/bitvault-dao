# 📦 BitVault DAO – Community Treasury Management Protocol

A **trustless, Bitcoin-backed treasury governance system** built on the **Stacks blockchain**, enabling communities to securely manage pooled capital, create funding proposals, and participate in on-chain voting with full transparency.

---

## 📖 Overview

**BitVault DAO** empowers communities to govern a shared treasury using **STX deposits** as the basis for governance power. Members lock STX into the protocol to receive governance tokens, which they can use to propose funding allocations and vote on initiatives. The protocol ensures that all fund movements are:

* **Community-driven**
* **Time-locked for security**
* **Verifiable on-chain**

This smart contract is fully written in [Clarity](https://docs.stacks.co/write-smart-contracts/clarity-overview), the safe and decidable language for Stacks smart contracts.

---

## 🏗️ System Architecture

```
+--------------------------+
|      BitVault DAO       |
|  (Clarity Smart Contract)|
+-----------+--------------+
            |
            v
+--------------------------+
|      Community Users     |
| - Deposit STX            |
| - Receive Voting Power   |
| - Propose & Vote         |
+--------------------------+
            |
            v
+--------------------------+
|     Governance Engine    |
| - Proposal Registry      |
| - Voting Tallying        |
| - Execution Constraints  |
+--------------------------+
            |
            v
+--------------------------+
|     Treasury Vault       |
| - STX Transfers          |
| - Time-locked Withdrawals|
+--------------------------+
```

---

## ⚙️ Contract Architecture

### 📌 Initialization

* `initialize`: One-time function callable by the contract owner to activate the DAO.

---

### 💰 Token Economics

* **STX Deposits:** Members deposit STX to participate.
* **Governance Tokens:** 1:1 minted against STX, non-transferable.
* **Withdrawal:** Allowed only after a configurable lock period.

#### Key Functions

* `deposit(amount)` – Lock STX and mint voting power.
* `withdraw(amount)` – Burn governance tokens and unlock STX after lock period.
* `get-balance(account)` – View token balance.

---

### 🗳️ Governance

Members with governance tokens can submit and vote on proposals.

#### Key Functions

* `create-proposal(description, amount, target, duration)`
  Propose STX allocation to an address with a set voting window.

* `vote(proposal-id, vote-for)`
  Cast a yes/no vote on active proposals.

* `execute-proposal(proposal-id)`
  Transfers funds to the recipient **only if**:

  * Proposal passed with majority
  * Voting window expired
  * Treasury has sufficient balance

---

## 🧠 Data Model & Storage

### 🔐 Global Variables

| Variable          | Type | Description                       |
| ----------------- | ---- | --------------------------------- |
| `initialized`     | bool | Protocol bootstrapped flag        |
| `total-supply`    | uint | Total governance tokens issued    |
| `proposal-count`  | uint | Running count of proposals        |
| `minimum-deposit` | uint | Minimum deposit threshold (1 STX) |
| `lock-period`     | uint | Time lock duration (default 10d)  |

---

### 📦 Maps

| Map         | Key Type    | Value Type                        | Purpose                          |
| ----------- | ----------- | --------------------------------- | -------------------------------- |
| `balances`  | principal   | uint                              | Voting power (minted tokens)     |
| `deposits`  | principal   | {amount, lock-until, last-reward} | Deposit and lock info            |
| `proposals` | uint        | Proposal struct                   | Active and past governance items |
| `votes`     | {id, voter} | bool                              | Prevents double voting           |

---

### 🛑 Error Codes

Defined with clarity for traceability:

* `u100` – Owner-only access
* `u101` – Not initialized
* `u103` – Insufficient balance
* `u107` – Proposal expired
* `u110` – Tokens still locked
* `u108` – Already voted
* ...and more

---

## 🔐 Security & Trust Assumptions

* **Time-locked withdrawals** prevent immediate exits post-voting.
* **Proposal execution** is gated by majority approval and expiration of voting window.
* **Contract owner** is only used during initialization.
* **Token minting/burning** is non-transferable to maintain fairness.

---

## 🧪 Testing & Deployment

You are advised to write unit tests using Clarinet or testnet scenarios to validate:

* Deposit & withdrawal logic
* Proposal lifecycle
* Voting mechanics
* Edge cases (e.g., duplicate votes, premature execution)

---

## 📘 Future Enhancements

* **Tokenization of Governance Power** for delegation
* **Reward distribution** based on participation
* **Multi-signature treasury integration**
* **Off-chain metadata for proposals**

---

## 📄 License

MIT License – Open-sourced for public good.

---

## 🙋‍♂️ Contributors

Crafted with care by senior Clarity developers. Feel free to fork, improve, and build upon it.
