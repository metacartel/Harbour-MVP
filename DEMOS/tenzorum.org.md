# Tenzorum Gasless TXs

### Format

```
{
      uint8 _v, bytes32 _r, bytes32 _s,
      address _from, //the subscriber
      address _to, //the publisher
      uint _value, //wei value of transfer
      bytes _data, //data input
      address _rewardType, //reward relayers in tokens or ETH
      uint _rewardAmount //amount to reward
}
```

### Rx
```
    ##Client SDK

    HTTP POST https://login.tenzorum.app/execute + {payload}
    GRAPHQL endpoint mutation ExecuteGaslessTx($from: $WalletAddress!, $to: $WalletAddress!, $amount: Int!) {
                       executeTransaction(from: $from, to: $to) {
                            amount
                       }
                     }

    ##Relay RPG
    - Coming
```

### Tx
```
{
    function execute(
      uint8 _v, bytes32 _r, bytes32 _s,
      address _from,
      address _to,
      uint _value,
      bytes _data,
      address _rewardType, uint _rewardAmount) public
}
```

### Example Implementation Links
- Coming

[Tenzorum](https://github.com/austintgriffith/meta-transaction-format-share/blob/master/tenzorum.org.md)
