# Public Rest API for Rokkex (2019-08-05)
# General API Information
* The base endpoint is: **https://api.rokkex.com**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests;
  the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on
  Rokkex's side.
  It is important to **NOT** treat this as a failure operation; the execution status is
  **UNKNOWN** and could have been a success.

* For `GET` endpoints, parameters must be sent as a `query string`.
* Parameters may be sent in any order.

# Public API Endpoints
## Terminology
* `base asset` refers to the asset that is the `quantity` of a symbol.
* `quote asset` refers to the asset that is the `price` of a symbol.


## ENUM definitions
**Symbol status (status):**

* AUCTION_MATCH
* BREAK
* END_OF_DAY
* HALT
* POST_TRADING
* PRE_TRADING
* TRADING

**Order status (status):**

* NEW
* OPEN
* PARTIALLY_FILLED
* FILLED
* CANCELED
* REJECTED
* EXPIRED

**Order types (orderTypes, type):**

* LIMIT
* MARKET

**Order side (side):**

* BUY
* SELL

**Kline/Candlestick chart intervals:**

m -> minutes; h -> hours; d -> days; w -> weeks; M -> months

* 1m
* 3m
* 5m
* 15m
* 30m
* 1h
* 2h
* 4h
* 6h
* 8h
* 12h
* 1d
* 3d
* 1w
* 1M

## Market Data endpoints
### Order book
```
GET /v1/public/depth
```


**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |

**Response:**
```javascript
{
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"    // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",
      "12.00000000"
    ]
  ]
}
```

### Recent trades list
```
GET /v1/public/trades
```
Get recent trades (up to last 500).

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 500

**Response:**
```javascript
[
  {
    "id": "7132540f-7313-40dc-9c40-46893966680f",
    "price": "4.00000100",
    "qty": "12.00000000",
    "time": 1499865549590,
    "side": "SELL",
    "isBuyerMaker": true
  }
]
```


### 24hr ticker price change statistics
```
GET /v1/public/ticker/24hr
```
24 hour rolling window price change statistics. **Careful** when accessing this with no symbol.

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |

* If the symbol is not sent, tickers for all symbols will be returned in an array.

**Response:**
```javascript
{
  "baseAsset": "BTC",
  "quoteAsset": "ETH",
  "volume": 36,
  "lowPrice": "0.10000000",
  "highPrice": "100.00000000",
  "lastPrice": "4.00000200",
  "count": "76"         // Trade count
}
```
OR
```javascript
[
  {
    "baseAsset": "BTC",
    "quoteAsset": "ETH",
    "volume": 36,
    "lowPrice": "0.10000000",
    "highPrice": "100.00000000",
    "lastPrice": "4.00000200",
    "count": "76"         // Trade count
  }
]
```
