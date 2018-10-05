# Argent

**Project Name**: 

- Argent Wallet

**Links of project**:

- https://www.argent.im
- https://medium.com/argenthq/decentralised-and-seedless-wallet-recovery-5fcf7dddd78d

## Code

### Meta-tx format

#### Format 
```
{
  address module, // the address of the wallet module that the relayer needs to call
  address wallet, // the address of the wallet on behalf of which the call is made
  bytes data, // the data of the call 
  uint256 nonce, // a nonce to prevent replayability of the transaction
  bytes signature // the concatenated signatures of the agents authorizing the transaction
  uint256 gasPrice, // the gasPrice to use to refund the relayer (0 if no refund required)
}
```


### Contract interface + Execution function
```
contract RelayerModule {
    function execute(
        address _wallet,
        bytes _data, 
        uint256 _nonce, 
        bytes _signatures, 
        uint256 _gasPrice
    )
        external
        returns (bool success);
}
contract SomeWalletModule is RelayerModule {

}
```



