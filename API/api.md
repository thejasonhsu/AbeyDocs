## Scan API Reference

List of APIs:

- [statusDatabase](#statusDatabase)
- [statusSupply](#statusSupply)

#### statusDatabase

Uri: /status/database

Request method: GET

Returns Abey's real-time data, including lightning blocks, block times, transaction Counts, address counts, and committee.

##### Parameters

Parameters: None

##### Returns

`lightningBlocks` - Lightning Block.

`snailBlocks` - Snail Block.

`totalTxs` - Transaction Count.

`totalAddress` - Address Count.

`committee` - Committee.

`blockTime` - Block Time.

##### Example
```js
// Request
curl --location --request GET 'https://api.abeyscan.com/api/status/database'

// Result
{
    "code": 200,
    "message": "success",
    "data": {
        "lightningBlocks": 8027675,
        "totalTxs": 33230820,
        "totalAddress": 163163,
        "snailBlocks": 67272,
        "committee": 322,
        "blockTime": "5.1"
    }
}
```
#### statusSupply

Uri: /status/supply

Request method: GET

Returns Abey's supply information, including market cap, price, max supply, total supply, and remaining supply. 

##### Parameters

Parameters: None

##### Returns

`marketCap` - Market Cap.

`remainingSupply` - Remaining Supply.

`totalSupply` - Total Supply.

`price` - Price.

`maxSupply` - Max Supply.

##### Example
```js
// Request
curl --location --request GET 'https://api.abeyscan.com/api/status/supply'

// Result
{
    "code": 200,
    "message": "success",
    "data": {
    "data": {
        "marketCap": 777774531.28204, 
        "remainingSupply": 234273111,
        "totalSupply": 1306832669,
        "price": 0.59516,
        "maxSupply": 1541105780
    }
}
}
```

