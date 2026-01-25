_Last updated: January 2026_

# Abey Staking Contract

## Contract Address

The Abey staking contract is deployed at the following address:

```
0x000000000000000000747275657374616b696E67
```

---

## Contract JSON ABI

Refer to the official documentation for the full ABI definition:

- [Staking ABI](https://github.com/abeychain/go-abey/wiki/Staking-ABI)

---

## Contract Interface

This section describes how validators and delegators interact with the staking contract.

---

## `deposit`

A node can participate as a validator and propose new blocks by calling the `deposit` function.

To create a validator, you must deposit a certain amount of `ABEY` and register your validator public key. Delegators may bond their tokens to a validator and share block rewards proportionally. Validators may charge an additional fee rate on delegator rewards.

### Fee Rate Calculation

```
feeRate = fee / 10000
```

After depositing, the node becomes a validator candidate. Only validators with a deposit balance greater than **20,000 ABAY** may be selected as active candidates.

### Parameters

| Name   | Type    | Description                                                         |
| ------ | ------- | ------------------------------------------------------------------- |
| pubkey | bytes   | BFT public key (65 bytes)                                           |
| fee    | uint256 | Percentage of reward charged to delegators (`fee / 10000`)          |
| value  | wei     | Amount of `abey` to deposit                                         |

---

## `cancel`

A validator may cancel (unstake) a portion of their deposited balance. Once canceled, the amount is locked in the contract for approximately **2 weeks** before it can be withdrawn.

### Parameters

| Name  | Type    | Description                                      |
| ----- | ------- | ------------------------------------------------ |
| value | uint256 | Amount to unlock from the staking balance (wei) |

---

## `append`

A validator may increase their staking balance by depositing additional `abey` using the `append` function.

### Parameters

| Name  | Type | Description                                   |
| ----- | ---- | --------------------------------------------- |
| value | wei  | Amount of additional `abey` to stake          |

---

## `withdraw`

A validator may withdraw unlocked tokens after the 2-week locking period. The full deposit state can be queried using the `getDeposit` function.

### Parameters

| Name  | Type         | Description                                   |
| ----- | ------------ | --------------------------------------------- |
| value | uint256 (wei) | Amount of unlocked tokens to withdraw        |

---

## `getDeposit`

Retrieves the deposit balance of a validator. Deposits exist in three states:

- **staked**: Tokens actively staked and eligible for rewards  
- **locked**: Tokens canceled but still in the lock period  
- **unlocked**: Tokens available for withdrawal  

### Parameters

| Name  | Type    | Description                         |
| ----- | ------- | ----------------------------------- |
| owner | address | Validator deposit owner address     |

### Return Values

Returns a tuple of three `uint256` values: `(staked, locked, unlocked)`

| Name     | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| staked   | uint256 | Amount currently staked                          |
| locked   | uint256 | Amount canceled but still locked                 |
| unlocked | uint256 | Amount available for withdrawal                  |

---

## `delegate`

Users who do not operate a validator node may delegate their tokens to an existing validator to earn staking rewards.

If a validator’s own deposit is less than **20,000 ABAY**, delegators will not receive rewards because the validator will not be elected.

### Parameters

| Name   | Type    | Description                                           |
| ------ | ------- | ----------------------------------------------------- |
| holder | address | Address of the validator being delegated to           |
| value  | wei     | Amount of `abey` delegated                            |

---

## `undelegate`

A delegator may cancel a portion of their delegated stake. The canceled amount is locked for approximately **2 weeks** before it becomes withdrawable.

### Parameters

| Name   | Type    | Description                                  |
| ------ | ------- | -------------------------------------------- |
| holder | address | Validator address                             |
| value  | uint256 | Amount of delegated stake to unlock (wei)   |

---

## `withdrawDelegate`

Delegators may withdraw unlocked tokens after the lock period. The delegation balance can be queried using the `getDelegate` function.

### Parameters

| Name   | Type    | Description                                   |
| ------ | ------- | --------------------------------------------- |
| holder | address | Validator address                             |
| value  | uint256 | Amount of unlocked tokens to withdraw         |

---

## `getDelegate`

Returns the delegation balance of a delegator. The balance states are identical to `getDeposit`.

### Parameters

| Name   | Type    | Description                                   |
| ------ | ------- | --------------------------------------------- |
| owner  | address | Owner of the delegated tokens                 |
| holder | address | Validator address                             |

### Return Values

Returns a tuple of three `uint256` values: `(staked, locked, unlocked)`

| Name     | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| staked   | uint256 | Amount currently delegated and earning rewards   |
| locked   | uint256 | Amount canceled but still locked                 |
| unlocked | uint256 | Amount available for withdrawal                  |
