# ENGINES

Engines in Peatio represent market's matching engine settings.

| column           | Desc                                                 |
| ---------------- | ---------------------------------------------------- |
| id               | an unique identifier in database                     |
| name             | human-readable description                           |
| driver           | More about driver in DRIVERS section                 |
| uid              | User's UID for upstream markets (more in UPSTREAMS)  |
| url              | URL for upstream                                     |
| key_encrypted    | apikey kid for upstream                              |
| secret_encrypted | apikey secret for upstream                           |
| data_encrypted   | can hold any engine-specific key-value configuration |
| state            | Either online or offline                             |

Every market belongs to one of defined engines and every engine should have one of supported drivers.

## DRIVERS

Drivers can be divided into two groups - Local or Upstream.

Supported local drivers are peatio and finex-spot.

Peatio comes with peatio driver available only.

Upstream drivers are supported in finex trading engine.

## UPSTREAM

Upstream is a remote platform you can connect to to use them as liquidity provider for your platform.

To setup upstream market, you need to create an upstream engine:

```ruby
engine = Engine.create(name: "BitFinex", driver: "bitfinex", uid: "UID", key: "KID", secret: "SECRET", url: "wss://api.bitfinex.com/ws/2")
```

`UID` is an 'upstream' user UID on **your platform**. When you submit an order to the upstream market and get a trade on remote platform, the trade will be recorded as a trade between you and this user.

`KID` and `SECRET` are your **remote platform** credentials.

Next step is to setup market's upstream config:

```ruby
Market.find("somemarket").update!(engine: engine, data: {upstream: {"driver": "bitfinex", "target": "market_on_remote_platform", "rest": "http://api-pub.bitfinex.com/ws/2", "websocket": "wss://api-pub.bitfinex.com/ws/2", "trade_proxy"=>true, "orderbook_proxy"=>true}})
```

`engine` will indicate that upstream driver should be used.

`data.upstream` enables liquidity proxy.

And do not forget to deposit some funds to remote platform :)
