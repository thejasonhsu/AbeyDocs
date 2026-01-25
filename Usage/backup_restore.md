_Last updated: January 2026_

# Backup and Restore Guide

This document explains how to safely back up, restore, and maintain data for the **Abey (`gabey`)** node, including accounts, blockchain data, and database maintenance.

---

## Data Directory

All persistent data used by `gabey` is stored in its **data directory** (with the exception of the PoW Ethash DAG, which is managed separately).

### Default Data Directory Locations

| Platform | Path |
|--------|------|
| macOS | `~/Library/abeyfoundation` |
| Linux | `~/.abeyfoundation` |
| Windows | `%APPDATA%\abeyfoundation` |

---

## Keystore (Critical)

Account private keys are stored in the `keystore` subdirectory inside the data directory.

- The `keystore` directory contains **encrypted private keys**
- These files are **portable** across:
  - Nodes
  - Platforms
  - Client implementations (Go, C++, Python)

✅ **Backing up the `keystore` directory is sufficient to recover your accounts**, provided you know the passwords.

❌ Losing both the keystore **and** the password results in permanent loss of funds.

---

## Custom Data Directory

You may specify a custom data directory using the `--datadir` flag:

```bash
gabey --datadir /custom/path
```

For all available CLI options, see:
- https://github.com/abeyfoundation/go-abey/wiki/Command-Line-Options

---

## Database Upgrades

Occasionally, internal database formats change between releases.

To upgrade the database format, ensure `gabey` is **not running**, then execute:

```bash
gabey upgradedb
```

This operation updates internal structures without affecting your keystore.

---

## Database Cleanup

To remove the blockchain and state databases:

```bash
gabey removedb
```

### Important Notes

- This deletes **blockchain and state data only**
- The `keystore` directory is **not affected**
- Useful when:
  - Resyncing from scratch
  - Switching networks
  - Fixing corrupted chain data

---

## Blockchain Export

You can export the blockchain to a binary file for backup or migration purposes.

### Export Full Chain

```bash
gabey export <filename>
```

### Export a Block Range

To export a specific block range (for example, the first epoch):

```bash
gabey export <filename> 0 29999
```

When exporting partial chains, data is **appended** to the output file rather than overwritten.

---

## Blockchain Import

Import a previously exported blockchain file using:

```bash
gabey import <filename>
```

This allows fast restoration without full resynchronization.

---

## Best Practices

- Back up your **keystore directory** regularly
- Store backups **offline** or in secure, encrypted storage
- Never share keystore files or passwords
- Stop `gabey` before performing upgrades or database operations
- Verify backups by restoring them on a test system

---

## Final Reminder

**DO NOT FORGET YOUR PASSWORD**  
**BACK UP YOUR KEYSTORE REGULARLY**
