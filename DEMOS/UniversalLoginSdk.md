# UniversalLoginSDK

**Project Name**: UniversalLoginSDK

**Links of project**:

- https://github.com/ethworks/universalLoginSDK/

**Short paragraph on where your team is at...**:

UniversalLoginSDK is an SDK composed of smart contracts, a js lib, and a relayer that help build applications using ERC1077 and ERC1078.

**Video DEMO Link:**

- https://www.youtube.com/watch?v=F5t94cCg6XE

## Code

### Meta-tx formatt - Eg. [Uport](https://github.com/Meta-tx/Harbour-MVP/blob/master/DEMOS/uport.org.md), [Tenzorum](https://github.com/Meta-tx/Harbour-MVP/blob/master/DEMOS/tenzorum.md)


#### Tx 
```js
/// @param to Destination address.
/// @param value Ether value.
/// @param data Data payload.
/// @param nonce Transaction nonce.
/// @param gasToken Token address (or 0 if ETH) that is used for the refund
/// @param gasPrice Gas price that should be used for this transaction.
/// @param gasLimit Maximum gas amount that should be used for this transaction.
/// @return Transaction nonce.
function executeSigned(
  address to, 
  uint256 value, 
  bytes data, 
  uint256 nonce, 
  address gasToken, 
  uint gasPrice, 
  uint gasLimit, 
  bytes messageSignatures)
```

We would like to extend it with following fields:
```
operationType
```

### Contract interface + Execution function - Eg. ...
```js
hash = keccak256(abi.encodePacked(
  byte(0x19), (tba)
  byte(0), (tba)
  from, 
  to, 
  value, 
  data, (dataHash tbu)
  nonce, 
  gasToken, 
  gasPrice, 
  gasLimit
)
```
To be added:
```
  callPrefix,
  operationType,
  extraHash
```

#### Interface 
```
function getKey(bytes32 _key) public view returns(uint256 purpose, uint256 keyType, bytes32 key);
function addKey(bytes32 _key, uint256 _purpose, uint256 _keyType) public returns (bool success);
function executeSigned(address to, uint256 value, bytes data, uint256 nonce, address gasToken, uint gasPrice, uint gasLimit, bytes messageSignatures)
fucntion lastNonce() public returns (uint nonce)
function requiredSignatures(uint type) returns (uint)
```

### REST API

POST /identity {managementKey, ensName}
POST /identity/execution {contractAddress, to, value, data, nonce, gasToken, gasPrice, gasLimit, signatures}

