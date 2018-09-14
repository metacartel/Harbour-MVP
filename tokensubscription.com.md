### Format
```
{
  address from, //the subscriber
  address to, //the publisher
  address tokenAddress, //the token address paid to the publisher
  uint256 tokenAmount, //the token amount paid to the publisher
  uint256 periodSeconds, //the period in seconds between payments
  uint256 gasPrice, //the amount of tokens or eth to pay relayer (0 for free)
  bytes signature //proof the subscriber signed the meta trasaction
}
```

### Rx
```
HTTP GET https://backend.metatx.io/subscriptions
```


### Tx
```
function executeSubscription(
        address from, //the subscriber
        address to, //the publisher
        address tokenAddress, //the token address paid to the publisher
        uint256 tokenAmount, //the token amount paid to the publisher
        uint256 periodSeconds, //the period in seconds between payments
        uint256 gasPrice, //the amount of tokens or eth to pay relayer (0 for free)
        bytes signature //proof the subscriber signed the meta trasaction
    )
        public
        returns (bool success)
```
Network: Mainnet Contract: 0x49748b0380d9370795CbD6809E52C927072107dC
