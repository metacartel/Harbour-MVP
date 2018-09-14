### Format 
```
{
  uint8 sigV,
  bytes32 sigR,
  bytes32 sigS,
  address destination,
  bytes data,
  address listOwner
}
```

### Rx 
```
{
  Coming Soon
}
```

### Tx
```
{
  // only allow senders from the whitelist specified by the user,
  // 0x0 means no whitelist.
  require(listOwner == 0x0 || whitelist[listOwner][msg.sender]);

  address claimedSender = getAddress(data);
  // use EIP 191
  // 0x19 :: version :: relay :: whitelistOwner :: nonce :: destination :: data
  bytes32 h = keccak256(byte(0x19), byte(0), this, listOwner, nonce[claimedSender], destination, data);
  address addressFromSig = ecrecover(h, sigV, sigR, sigS);

  require(claimedSender == addressFromSig);

  nonce[claimedSender]++; //if we are going to do tx, update nonce

  require(destination.call(data));
}
```
