# SNT Gas Relay

**Links of project**:

- https://github.com/status-im/snt-gas-relay
- https://ideas.status.im/ideas/150-gas-abstraction

With the goals of simplifying the onboarding experience of users at Status.im, the idea of the gas relay network was created to have a series of nodes connected to Whisper, and competing between them for processing transactions, being able to accept specific tokens as a reward for the gas fees spent for processing transactions

**Operation**
1. User sends an availability request to a whisper symmetric key (equivalent to a public channel), containing the details of the transaction: contract to execute, function, gas price, gas token
2. Relayers will analyze the request and if they accept the gas token, price and trust the contract, will reply back indicating their availability and min gas price accepted.
3. In the user end, they will pick a relayer that has replied back, and proceed to send a whisper message to the relayer, with the same parameters and specifying the transaction data (to, value data)
4. Once the relayer receives the transaction, it will perform the following validations:
a. Validate the contract to invoke is known by the relayer
b. Validate the gas token is accepted by the relayer and the gas price is greater or equal than the minimum accepted
c. Validation of balances / estimation of gas
d. Validation of transaction via estimation or simulation through Ganache
5. If the transaction is valid, it will be broadcasted and a whisper message with the transaction hash will be sent back to the user
6. Once the transaction is mined, a transaction receipt will be sent via whisper to the user

For these operations, two contracts were developed:
- **[SNTController](https://github.com/status-im/snt-gas-relay/blob/master/test-dapp/contracts/status/SNTController.sol)**: This contract is intended to be the owner of [SNT](https://etherscan.io/token/0x744d70fdbe2ba4cf95131626614a1763df805b9e) (a MiniMeToken). This contract allows the etherless transfer of SNT among parties, and contracts execution (this part intended to be used to generate new Identities via a factory)
- **[IdentityGasRelay](https://github.com/status-im/snt-gas-relay/blob/master/test-dapp/contracts/identity/IdentityGasRelay.sol)**: Provides additional meta-transaction related functions to our implementation of ERC725/735 - Identities & Claims


**How do we prevent a malicious relayer from either censoring a DApp they don't like or compete with, or just costing them gas by submitting a TX and then simultaneously submitting the same tx to their own relayer node, paying a slightly higher gas fee and then the honest relayers TX will fail and they will lose gas. It seems having miners act as relayers will solve this problem, but we don't want just miners to run the software right, any desktop client should be able to do it?**
Our gas relayer does not work like that, we are doing gas abstraction, and it's decentralized, therefore it don't matter what a gas relayer thinks, their only incentive is to include a transaction for getting SNT (or other erc20 that gas relayer have interest) for it.

**What about state channels with deterministic contract addresses for handling gas-less transactions? So, we agree that the state channel contract will be at 0xdeadbeef, then - as soon as some funds show up there, I as the operator deploy the contract and open the channel that the user can then use to send tokens without paying gas at any point?**

State channels can be used with gas relayer just if they would be used with ether gas. We are doing gas abstraction in top of account contract, so everything that works with EOA+ethergas works with Identity+ERC20gas. You basically could use any erc20 to open a state channel and transfer those tokens there, all only ever holding SNT.


**Would it not be possible for a user to submit a tx to an honest relayer node, and then submit that same tx to their own, malicious relayer node, have their node broadcast the tx at a higher gas fee, get it included first, and then the honest relayer's tx will fail and they will lose the gas they spent without receiving any tokens?**

We don't solve that problem right now. We expect to solve that by integrating the gas relayer in PoS. The gas cost wasted in the case mentioned would be low. To avoid that, gas relayers would black list addresses that behave like this. Integrating with PoS means validators themselves include that transactions using ether gas price zero. At the moment we will not deal with this, unless it proven to be a problem. The most natural solution is to, instead "transaction signer" making a signature than anyone can include, it would select one of many and make a signature specifically to that. There, we might not need a blocklist, while the "transaction signer" can still sign the same nonce to multiple accounts, however then we can punish the transaction signer.



**Video DEMO Link:**

- https://youtu.be/8AeIe2YMKnk

**Video DEMO summary**

- On the video it's shown the process of creating a etherless transaction for the transfer of tokens and a contract function invokation.

## Code

### Meta-tx format

#### Format 

Signed message hashes are built via the following functions:

##### SNTController
```
/**
 * @notice get execution hash
 * @param _allowedContract address of a contract in execution trust list;
 * @param _data msg.data to be sent to `_allowedContract`
 * @param _nonce current signNonce of message signer
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasMinimal minimal amount of gas needed to complete the execution
 */
function getExecuteGasRelayedHash(
    address _allowedContract,
    bytes _data,
    uint256 _nonce,
    uint256 _gasPrice,
    uint256 _gasMinimal
) public view returns (bytes32 execHash);

/**
 * @notice get transfer hash
 * @param _to address receving the tokens from message signer
 * @param _amount total being transfered
 * @param _nonce current signNonce of message signer
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 */
function getTransferSNTHash(
    address _to,
    uint256 _amount,
    uint256 _nonce,
    uint256 _gasPrice
) public view returns (bytes32 txHash);
```

##### IdentityGasRelay
```
/**
 * @notice get callGasRelay Hash
 * @param _to destination of call
 * @param _value call value (ether)
 * @param _dataHash call data hash
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender` 
 * @return callGasRelayHash the hash to be signed by wallet
 */
function callGasRelayHash(
    address _to,
    uint256 _value,
    bytes32 _dataHash,
    uint _nonce,
    uint256 _gasPrice,
    uint256 _gasLimit,
    address _gasToken
) public view returns (bytes32 _callGasRelayHash);

/**
 * @notice get deployGasRelay Hash
 * @param _value call value (ether)
 * @param _dataHash call data hash
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender` 
 * @return callGasRelayHash the hash to be signed by wallet
 */
function deployGasRelayHash(
    uint256 _value,
    bytes32 _dataHash,
    uint256 _nonce,
    uint256 _gasPrice,
    uint256 _gasLimit,
    address _gasToken
) public view returns (bytes32 _callGasRelayHash);

/**
 * @notice get approveAndCallHash
 * @param _to destination of call
 * @param _value call value (ether)
 * @param _dataHash call data hash
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender` 
 * @return callGasRelayHash the hash to be signed by wallet
 */
function approveAndCallGasRelayHash(
    address _baseToken,
    address _to,
    uint256 _value,
    bytes32 _dataHash,
    uint _nonce,
    uint256 _gasPrice,
    uint256 _gasLimit,
    address _gasToken
) public view returns (bytes32 _callGasRelayHash);

```

#### Tx 

- [Protocol](https://github.com/status-im/snt-gas-relay/blob/master/relayer-protocol.md)
- [JS library for building mrelayer whisper comms](https://github.com/status-im/snt-gas-relay/blob/master/javascript-library.md)

#### Rx
TODO - Documentation for transaction receipts is still in progress
    
### Contract interface + Execution function

#### SNTController

```
/** 
 * @notice allows externally owned address sign a message to transfer SNT and pay for the fees in SNT
 * @param _to address receving the tokens from message signer
 * @param _amount total being transfered
 * @param _nonce current signNonce of message signer
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _signature concatenated rsv of message
 */
function transferSNT(
    address _to,
    uint256 _amount,
    uint256 _nonce,
    uint256 _gasPrice,
    bytes _signature
) external;

 /**
 * @notice allows externally owned address sign a message to offer SNT for an execution 
 * @param _allowedContract address of a contracts in execution trust list;
 * @param _data msg.data to be sent to `_allowedContract`
 * @param _nonce current signNonce of message signer
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasMinimal minimal amount of gas needed to complete the execution
 * @param _signature concatenated rsv of message
 */
function executeGasRelayed(
    address _allowedContract,
    bytes _data,
    uint256 _nonce,
    uint256 _gasPrice,
    uint256 _gasMinimal,
    bytes _signature
) external;

```

#### IdentityGasRelay

```
/**
 * @notice include ethereum signed callHash in return of gas proportional amount multiplied by `_gasPrice` of `_gasToken`
 *         allows identity of being controlled without requiring ether in key balace
 * @param _to destination of call
 * @param _value call value (ether)
 * @param _data call data
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender`
 * @param _messageSignatures rsv concatenated ethereum signed message signatures required
 */
function callGasRelayed(
    address _to,
    uint256 _value,
    bytes _data,
    uint _nonce,
    uint _gasPrice,
    uint _gasLimit,
    address _gasToken, 
    bytes _messageSignatures
);
    
/**
 * @notice deploys contract in return of gas proportional amount multiplied by `_gasPrice` of `_gasToken`
 *         allows identity of being controlled without requiring ether in key balace
 * @param _value call value (ether) to be sent to newly created contract
 * @param _data contract code data
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender`
 * @param _messageSignatures rsv concatenated ethereum signed message signatures required
 */
function deployGasRelayed(
    uint256 _value, 
    bytes _data,
    uint _nonce,
    uint _gasPrice,
    uint _gasLimit,
    address _gasToken, 
    bytes _messageSignatures
) external returns(address deployedAddress);
        
/**
 * @notice include ethereum signed approve ERC20 and call hash 
 *         (`ERC20Token(baseToken).approve(_to, _value)` + `_to.call(_data)`).
 *         in return of gas proportional amount multiplied by `_gasPrice` of `_gasToken`
 *         fixes race condition in double transaction for ERC20.
 * @param _baseToken token approved for `_to`
 * @param _to destination of call
 * @param _value call value (in `_baseToken`)
 * @param _data call data
 * @param _nonce current identity nonce
 * @param _gasPrice price in SNT paid back to msg.sender for each gas unit used
 * @param _gasLimit minimal gasLimit required to execute this call
 * @param _gasToken token being used for paying `msg.sender`
 * @param _messageSignatures rsv concatenated ethereum signed message signatures required
 */
function approveAndCallGasRelayed(
    address _baseToken, 
    address _to,
    uint256 _value,
    bytes _data,
    uint _nonce,
    uint _gasPrice,
    uint _gasLimit,
    address _gasToken,
    bytes _messageSignatures
) external returns(bool success);
```
