# Gnosis Safe

### Project Name:

- Gnosis Safe

### Links of project:

- Smart contracts: https://github.com/gnosis/safe-contracts
- Clients: [iOS app](https://github.com/gnosis/safe-ios), [Android app](https://github.com/gnosis/safe-android), [Chrome extension](https://github.com/gnosis/safe-browser-extension), [Team Safe](https://github.com/gnosis/safe-react)
- Relay service: https://github.com/gnosis/safe-relay-service
- Docs: https://gnosis-safe.readthedocs.io/en/latest/

### About the project & status:

The Gnosis Safe is a multi-signature wallet that allows all users to safely store Ether and tokens with two-factor authentication, manage their funds and interact with decentralized applications on Ethereum. 

Meta transactions via a relay service are used in order to pay for transaction fees with funds from within the smart contract.

Current status: [Android app](https://play.google.com/apps/testing/pm.gnosis.heimdall.dev), [Chrome extension](https://chrome.google.com/webstore/detail/gnosis-safe-rinkeby/gkiklnclpcbphbiaickiepnnnahefkoc) & [relay service](http://safe-relay.rinkeby.gnosis.pm/) released on Rinkeby.

### Video DEMO Link:

- https://www.youtube.com/watch?v=XYnT54dE-vc


### Video DEMO summary:

The video show a walkthrough of how to setup a Safe and make transactions via the Safe relay service according to the tutorial on https://github.com/gnosis/safe-demo.

On the Safe relay service, there are multiple endpoints on this service for the purpose of deploying a Safe contract, estimating transactions and submitting transactions.

As all the parameters required for execution are part of the submitted transaction it is possible that miners front-run the original relayer to receive the reward. In the long run that behaviour would be appreciated, since it would allow that anybody submits these transactions with `gasPrice` of the transaction triggering `execTransaction` set to 0. Miners could pick up these transactions and claim the rewards. This is why it is possible to specify the account that should receive the refund.

The vision for submitting Safe transactions would be the following:

1. Acquire estimates for a Safe transaction from potential relayers
1. Choose a relayer and generate the signatures for the Safe transaction (the chosen relayer should be set to receive the rewards to prevent front-running)
1. Submit the Safe transaction with all required paramters to the chosen relayer.

#### Rationale of parameters
The `execTransaction` has quite some parameters which might be unnecessary for other projects that want to use a relay service. To better understand why this set of paramters was chosen the rationale of them will be outlined.

##### `operation`
The Safe contract allows the execution different types of transactions. These are differentiated by the operation. Currently the Safe supports three different types of transactions: `CALL`, `DELEGATECALL` and `CREATE`.

##### `safeTxGas`
When a relayer submits a transaction with valid signatures it should be paid even if the Safe transaction fails. This has been done for the following reasons:

1. If the transaction fails the signatures stay valid. Therefore it would be possible to potentially replay the transaction.
1. The relayer should always be paid even if the Safe transaction fails (e.g. due to state changes)

Is is necessary that the relayer cannot make the Safe transaction to fail on purpose. This would make it possible that the relayer gets paid without performing the Safe transaction. For this the client needs to specify the minimum required gas for the Safe transaction. This is similar to the gas limit of a normal Ethereum transaction.

##### `dataGas`
As outlined before `safeTxGas` only specifies how much gas should be available for the Safe transaction. The gas that the client needs to pay for is determined at runtime. But there are some static costs. This includes the costs for the payload and the gas required to perform the payment transfer.

##### `refundReceiver`
It is possible to specify the receiver of the refund to avoid that submitted transactions can be front-run by others.

### Code

##### Format 
Please also check the Swagger docs at https://safe-relay.rinkeby.gnosis.pm/.

```
{
    safe: string,
    to: string,
    value: integer,
    data  : string,
    operation: integer,
    safeTxGas: integer,
    dataGas  : integer,
    gasPrice : integer,
    gasToken  : string,
    refundReceiver: string,
    nonce: integer,
    signatures: [
        {
            v: integer,
            r: integer,
            s: integer
        },
        ...
    ]
}
```

##### Contract interface + Execution function
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

```
    /// @dev Allows to execute a Safe transaction confirmed by required number of owners and then pays the account that submitted the transaction.
    ///      Note: The fees are always transfered, even if the user transaction fails. 
    /// @param to Destination address of Safe transaction.
    /// @param value Ether value of Safe transaction.
    /// @param data Data payload of Safe transaction.
    /// @param operation Operation type of Safe transaction.
    /// @param safeTxGas Gas that should be used for the Safe transaction.
    /// @param dataGas Gas costs for data used to trigger the safe transaction and to pay the payment transfer
    /// @param gasPrice Gas price that should be used for the payment calculation.
    /// @param gasToken Token address (or 0 if ETH) that is used for the payment.
    /// @param refundReceiver Address of receiver of gas payment (or 0 if tx.origin).
    /// @param signatures Packed signature data ({bytes32 r}{bytes32 s}{uint8 v})
    function execTransaction(
        address to, 
        uint256 value, 
        bytes data, 
        Enum.Operation operation, 
        uint256 safeTxGas,
        uint256 dataGas,
        uint256 gasPrice,
        address gasToken,
        address refundReceiver,
        bytes signatures
    )
```

##### Calls and messages
Please check https://github.com/gnosis/safe-demo for details on calls and messages.
