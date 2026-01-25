_Last updated: January 2026_

# Abey Staking ABI

This document describes the **events** and **functions** exposed by the Abey staking contract ABI.

---

## Events

### `Deposit`

Emitted when a validator deposits stake and registers a public key.

| Name   | Type    | Indexed | Description                      |
|------- |-------- |-------- |----------------------------------|
| from   | address | true    | Validator address                |
| pubkey | bytes   | false   | Validator BFT public key         |
| value  | uint256 | false   | Amount deposited                 |
| fee    | uint256 | false   | Validator fee rate               |

---

### `Delegate`

Emitted when a delegator delegates stake to a validator.

| Name   | Type    | Indexed | Description                      |
|------- |-------- |-------- |----------------------------------|
| from   | address | true    | Delegator address                |
| holder | address | true    | Validator address                |
| value  | uint256 | false   | Amount delegated                 |

---

### `Undelegate`

Emitted when a delegator cancels delegated stake.

| Name   | Type    | Indexed | Description                      |
|------- |-------- |-------- |----------------------------------|
| from   | address | true    | Delegator address                |
| holder | address | true    | Validator address                |
| value  | uint256 | false   | Amount undelegated               |

---

### `WithdrawDelegate`

Emitted when a delegator withdraws unlocked stake.

| Name   | Type    | Indexed | Description                      |
|------- |-------- |-------- |----------------------------------|
| from   | address | true    | Delegator address                |
| holder | address | true    | Validator address                |
| value  | uint256 | false   | Amount withdrawn                 |

---

### `Cancel`

Emitted when a validator cancels a portion of their stake.

| Name  | Type    | Indexed | Description                      |
|------ |-------- |-------- |----------------------------------|
| from  | address | true    | Validator address                |
| value | uint256 | false   | Amount canceled                  |

---

### `Withdraw`

Emitted when a validator withdraws unlocked stake.

| Name  | Type    | Indexed | Description                      |
|------ |-------- |-------- |----------------------------------|
| from  | address | true    | Validator address                |
| value | uint256 | false   | Amount withdrawn                 |

---

### `Append`

Emitted when a validator adds additional stake.

| Name  | Type    | Indexed | Description                      |
|------ |-------- |-------- |----------------------------------|
| from  | address | true    | Validator address                |
| value | uint256 | false   | Amount appended                  |

---

### `SetFee`

Emitted when a validator updates their fee rate.

| Name | Type    | Indexed | Description               |
|----- |-------- |-------- |---------------------------|
| from | address | true    | Validator address         |
| fee  | uint256 | false   | New fee rate              |

---

### `SetPubkey`

Emitted when a validator updates their public key.

| Name   | Type    | Indexed | Description              |
|------- |-------- |-------- |--------------------------|
| from   | address | true    | Validator address        |
| pubkey | bytes   | false   | New BFT public key       |

---

## Functions

### `deposit(bytes pubkey, uint256 fee, uint256 value)`

Registers a validator by depositing stake and setting a public key.

---

### `setFee(uint256 fee)`

Updates the validator’s fee rate.

---

### `setPubkey(bytes pubkey)`

Updates the validator’s BFT public key.

---

### `append(uint256 value)`

Adds additional stake to an existing validator deposit.

---

### `delegate(address holder, uint256 value)`

Delegates stake to a validator.

---

### `undelegate(address holder, uint256 value)`

Cancels a portion of delegated stake.

---

### `cancel(uint256 value)`

Cancels a portion of the validator’s own stake.

---

### `withdraw(uint256 value)`

Withdraws unlocked validator stake.

---

### `withdrawDelegate(address holder, uint256 value)`

Withdraws unlocked delegated stake.

---

## View Functions

### `lockedBalance(address owner) → uint256`

Returns the total locked balance for an address.

---

### `getDeposit(address owner) → (uint256 staked, uint256 locked, uint256 unlocked)`

Returns the validator deposit state.

---

### `getDelegate(address owner, address holder) → (uint256 delegated, uint256 locked, uint256 unlocked)`

Returns the delegation state between a delegator and a validator.
