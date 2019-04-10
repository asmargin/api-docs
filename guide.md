# Asmargin HTTP API Documentation (v1)
## General Information
* The base endpoint is: https://www.asmargin.com/api/v1/
* All endpoints return either a JSON object or array.
* All endpoints use HTTPS.
* Public endpoints must use `GET` method.
* Private endpoints must use `POST` method and contain the following headers and values:

`API-Key`: The API key  
`API-Signature`: Message signature using HMAC-SHA256 of (nonce + URI path + POST data) and base64 decoded secret API as the key  
`API-Nonce`: An ever-increasing 64-bit integer for this API key. Note that there is no way to reset the nonce to a lower value. It is recommended that you use a nonce generation method that will not generate numbers less than the previous nonce. Milliseconds since Unix epoch or a persistent counter can be used.

> Base64-decode the API secret string before using it as the key for HMAC. Decoding the string must result in a 32-byte key.

* All responses are `application/json` content type.
* For `POST` endpoints, the parameters must be sent in the request body with content type `application/x-www-form-urlencoded` and parameters can be sent in any order.
* Any endpoint can return an error. If the response payload includes `code` field, it is an error. For example:
```javascript
{
  "code": -100,
  "message": "Invalid pair."
}
```

## Public Endpoints
```
/orderBook
```
Returns the order book data for the pair specified.

Parameter | Type    | Description
--------- | ------- | ---------------
pair      | string  |

```
/ticker
```
Returns the ticker information for all pairs.


```
/ohlc
```
Returns candlestick bars for the pair specified.

Parameter | Type    | Description
--------- | ------- | ---------------
pair      | string  |
interval  | string  | Valid values are `1m`, `5m`, `15m`, `1h`, `2h`, `4h`, `6h`, `1d`

```
/trades
```
Returns the most recent trades for the pair specified.

Parameter | Type    | Description
--------- | ------- | ---------------
pair      | string  |


## Private Endpoints
```
/balances
```
Returns your balances for each currency. Empty balances are not returned.

```
/openPosition
```
Opens a margin position and returns the position ID.

Parameter | Type    | Description
--------- | ------- | ---------------
pair      | string  |
type      | string  | `long` or `short`
amount    | decimal | Margin amount
leverage  | number  |
stoploss  | decimal | Optional

```
/closePosition
```
Closes a position and realizes the profit/loss.

Parameter    | Type    | Description
------------ | ------- | ---------------
position_id  | string  |

```
/positions
```
Returns your most recent 100 positions.
```
/placeOrder
```
Places an order and returns the order ID.

Parameter | Type    | Description
--------- | ------- | ---------------
pair      | string  |
type      | string  | `limit` or `market`
side      | string  | `buy` or `sell`
amount    | decimal | Order amount in base currency
price     | decimal | Required for limit orders only


```
/cancelOrder
```
Cancels an unfilled order and returns the unfilled part to your available balance.

Parameter | Type    | Description
--------- | ------- | ---------------
order_id  | string  |

```
/orders
```
Returns your most recent 100 orders.

```
/depositAddress
```
Returns the deposit address for the currency requested.

Parameter | Type    | Description
--------- | ------- | ---------------
currency  | string  |


```
/withdraw
```
Submits a withdrawal request for processing.

Parameter | Type    | Description
--------- | ------- | ---------------
currency  | string  |
address   | string  |
tag       | string  | Required for XMR and XRP only
amount    | decimal |

```
/holds
```
Returns the active holds. Holds are placed on an account for any open orders/positions or pending withdrawal requests.
