### Format
```
    /// @dev Returns hash to be signed by owners.
    /// @param to Destination address.
    /// @param value Ether value.
    /// @param data Data payload.
    /// @param operation Operation type.
    /// @param safeTxGas Fas that should be used for the safe transaction.
    /// @param dataGas Gas costs for data used to trigger the safe transaction.
    /// @param gasPrice Maximum gas price that should be used for this transaction.
    /// @param gasToken Token address (or 0 if ETH) that is used for the payment.
    /// @param refundReceiver Address of receiver of gas payment (or 0 if tx.origin).
    /// @param _nonce Transaction nonce.
    /// @return Transaction hash.
    function getTransactionHash(
        address to, 
        uint256 value, 
        bytes data, 
        Enum.Operation operation, 
        uint256 safeTxGas, 
        uint256 dataGas, 
        uint256 gasPrice, 
        address gasToken,
        address refundReceiver,
        uint256 _nonce
    )
```

### Rx
```
There is currently no endpoint to pull the transactions from.
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

Please check https://github.com/gnosis/safe-contracts for further details and context.