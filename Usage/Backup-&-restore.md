**DO NOT FORGET YOUR PASSWORD** and **BACKUP YOUR KEYSTORE**

# Backup & restore

## Data directory

Everything `gabey` persists gets written inside its data directory (except for the PoW Ethash DAG, see note below).
The default data directory locations are platform specific:

* Mac: `~/Library/abeyfoundation`
* Linux: `~/.abeyfoundation`
* Windows: `%APPDATA%\abeyfoundation`

Accounts are stored in the `keystore` subdirectory. The contents of this directories should be transportable between nodes, platforms, implementations (C++, Go, Python).

To configure the location of the data directory, the `--datadir` parameter can be specified. See [CLI Options](https://github.com/abeyfoundation/go-abey/wiki/Command-Line-Options) for more details.

## Upgrades

Sometimes the internal database formats need updating. This can be run with the following command (gabey should not be otherwise running):

```
gabey upgradedb
```

## Cleanup

gabey's blockchain and state databases can be removed with:

```
gabey removedb
```

This is useful for deleting an old chain and sync'ing to a new one. It only affects data directories that can be re-created on synchronisation and does not touch the keystore.

## Blockchain import/export

Export the blockchain in binary format with:
```
gabey export <filename>
```

Or if you want to back up portions of the chain over time, a first and last block can be specified. For example, to back up the first epoch:

```
gabey export <filename> 0 29999
```

Note that when backing up a partial chain, the file will be appended rather than truncated.

Import binary-format blockchain exports with:
```
gabey import <filename>
```


And finally: **DO NOT FORGET YOUR PASSWORD** and **BACKUP YOUR KEYSTORE**
