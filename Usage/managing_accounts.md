_Last updated: January 2026_

# Managing Your Accounts

If you lose the password used to encrypt an account, **you permanently lose access to that account**.  
There is **no password recovery mechanism**.  

This guide explains how to manage Abey accounts using the **`gabey` CLI**, including creating, listing, importing, updating, unlocking accounts, and checking balances.

---

## Overview

Account management is handled via the `account` command:

```bash
gabey account <command> [options] [arguments]
```

Supported account operations include:
- Creating new accounts
- Listing existing accounts
- Importing private keys
- Updating account formats and passwords
- Unlocking accounts for transactions

Accounts are stored in the **keystore directory**, which must be backed up regularly.

---

## Keystore and Security

- Account keys are stored in:  
  ```
  <DATADIR>/keystore
  ```
- Keystore files are **encrypted**
- Files are portable across:
  - Nodes
  - Operating systems
  - Abey client implementations

Backing up the keystore directory is sufficient to restore accounts **only if the password is known**.

For data directory details, see:
- https://github.com/abeyfoundation/go-abey/wiki/Backup-&-restore

---

## Account File Format

Modern keystore files use the format:

```
UTC--<ISO8601 timestamp>-<address>
```

- Accounts are listed **lexicographically**
- Due to timestamp-based filenames, this corresponds to **creation order**
- Copying keys between nodes may change account ordering

Do **not** rely on account index ordering in scripts.

---

## Account Commands

| Command | Description |
|------|-------------|
| `list` | List existing accounts |
| `new` | Create a new account |
| `import` | Import a private key |
| `update` | Update account password or format |

Use `gabey account <command> --help` for command-specific options.

---

## Creating Accounts

### Interactive Account Creation

```bash
gabey account new
```

You will be prompted for a password:

```text
Passphrase:
Repeat Passphrase:
Address: {168bc315a2ee09042d83d7c5811b533620531f67}
```

---

### Non-Interactive Account Creation (Advanced)

```bash
gabey account new --password /path/to/passwordfile
```

This is intended for **testing only**.  
Never store plaintext passwords insecurely.

---

### Create via Console

```javascript
personal.newAccount("passphrase")
```

---

## Importing a Private Key

```bash
gabey account import <keyfile>
```

- `<keyfile>` must contain a **raw, unencrypted EC private key (hex)**
- The imported account is stored encrypted
- You will be prompted for a password

Non-interactive import:

```bash
gabey account import --password /path/to/passwordfile <keyfile>
```

Import is **not required** when moving encrypted keystore files between nodes.

---

## Updating an Existing Account

Update password or migrate account format:

```bash
gabey account update <address|index> [<address|index> ...]
```

Examples:

```bash
gabey account update 0 1
gabey account update a94f5374fce5edbc8e2a8697c15331677e6ebf0b
```

- Prompts for old and new passwords
- Old key formats are removed after update

---

## Listing Accounts

```bash
gabey account list
```

Example output:

```text
Account #0: {5afdd78b...} keystore:///.../UTC--2017-04-28...
Account #1: {9acb9ff9...} keystore:///.../UTC--2017-04-28...
```

### Using RPC

```bash
curl -X POST   --data '{"jsonrpc":"2.0","method":"abey_accounts","params":[],"id":1}'   http://127.0.0.1:8545
```

---

## Unlocking Accounts

### Command-Line Unlock (Session Only)

```bash
gabey --unlock "0xabc...,0,2" --password /path/to/passwordfile
```

- Accepts addresses or indexes
- Multiple accounts allowed
- Password file must contain one password per line

---

### Console Unlock (Temporary)

```javascript
personal.unlockAccount(address, "password", 300)
```

Avoid passing passwords directly in console history.

---

## Checking Account Balances

### Coinbase Balance

```javascript
web3.fromWei(abey.getBalance(abey.coinbase), "abey")
```

---

### All Account Balances

```javascript
function checkAllBalances() {
    var total = 0;
    for (var i in abey.accounts) {
        var bal = web3.fromWei(abey.getBalance(abey.accounts[i]), "abey");
        total += parseFloat(bal);
        console.log(i, abey.accounts[i], bal);
    }
    console.log("Total:", total);
}
```

Run with:

```javascript
checkAllBalances();
```

---

## Best Practices

- Back up the keystore directory regularly
- Never share passwords or keystore files
- Avoid plaintext password files
- Prefer IPC for account operations
- Test scripts on non-production nodes
