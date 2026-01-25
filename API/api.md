Updated as of January 2026

## Scan API Reference

List of APIs:

- [stats](#stats)
- [statsSupply](#statsSupply)

#### stats

Uri: /v2/stats

Request method: GET

Returns Abey's real-time data, including lightning blocks, block times, transaction Counts, address counts, and committee.

##### Parameters

Parameters: None

##### Example
```js
// Request
curl --location --request GET 'https://api.abeyscan.com/api/v2/stats'

// Result
{
    "average_block_time": 5111.0,
    "coin_image": null,
    "coin_price": "0.03221",
    "coin_price_change_percentage": -2.24,
    "gas_price_updated_at": "2026-01-25T01:45:48.457777Z",
    "gas_prices": {
        "slow": 13.0,
        "average": 13.0,
        "fast": 13.0
    },
    "gas_prices_update_in": 5961,
    "gas_used_today": "137710194",
    "network_utilization_percentage": 0.017934125,
    "total_addresses": "357207",
    "total_blocks": "29947767",
    "total_gas_used": "0",
    "total_transactions": "74812188",
    "transactions_today": "6200",
}
```
#### statsSupply

Uri: /stats/supply

Request method: GET

Returns Abey's current total supply. 

##### Parameters

Parameters: None

##### Example
```js
// Request
curl --location --request GET 'https://api.abeyscan.com/api/stats/supply'

// Result
{
    "message": "OK",
    "result": "1392562419278214324414305317",
    "status": "1"
}
```

