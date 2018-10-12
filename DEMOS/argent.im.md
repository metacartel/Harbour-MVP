# Argent

**Project Name**: 

- Argent Wallet

**Links**:

- https://www.argent.im
- https://medium.com/argenthq/decentralised-and-seedless-wallet-recovery-5fcf7dddd78d

### Meta-tx format
```
{
  address from, // the address of the wallet module contract that the relayer needs to call
  address to, // the address of the contract the wallet module should call (to == from in Argent architecture)
  address wallet, // the address of the wallet on behalf of which the call is made
  bytes data, // the data of the call made by the wallet module contract
  uint256 nonce, // a nonce to prevent replayability of the transaction
  uint256 value, // the value in wei the wallet module needs to attach to the transaction (always 0 for Argent)
  bytes signature // the concatenated signatures of the agents authorizing the transaction
  uint256 gasPrice, // the gasPrice to use to refund the relayer (0 if no refund required)
}
```

### Meta-tx validation by the relayer
Argent relayer expects to receive relay request via a JSON RPC protocol implementing a single method `eth_relayTransaction`. The relayer currently performs the following validation:

1. Verify that the received JSON RPC `params` object is a single element array containing a JSON object with at least the fields `from`, `wallet`, `data`, `nonce` and `signatures`, and optionally the fields `to`, `value` and `gasPrice`.
2. Verify that `to == from` if `to` is present in the JSON object.
3. Verify that `value == 0` if `value` is present in the JSON object.
4. Verify that either `gasPrice > 0` or `from` is a registered Argent Wallet Module (checked by making a call to a ModuleRegistry contract).

If the meta-tx is deemed valid, the relayer submits it to Infura and returns the transaction hash back to the user.

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
