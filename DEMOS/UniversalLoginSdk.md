# UniversalLoginSDK

**Project Name**: UniversalLoginSDK

**Links of project**:

- https://github.com/ethworks/universalLoginSDK/

**Short paragraph on where your team is at...**:

UniversalLoginSDK is an SDK composed of smart contracts, a js lib, and a relayer that help build applications using ERC1077 and ERC1078.

**Video DEMO Link:**

- https://www.youtube.com/watch?v=F5t94cCg6XE

## Code

#### Tx 
```js
/// @param to Destination address.
/// @param value Ether value.
/// @param data Data payload.
/// @param nonce Transaction nonce.
/// @param gasToken Token address (or 0 if ETH) that is used for the refund
/// @param gasPrice Gas price that should be used for this transaction.
/// @param gasLimit Maximum gas amount that should be used for this transaction.
/// @param operationType 0 - call, 1 - delegate call, 2 - create
/// @param extraHash - for future compatibility (for now always keccak256(bytes32(0x0)))
/// @return execution idetifier: signature hash, keccak256(signatures)
function executeSigned(
  address to, 
  uint256 value, 
  bytes data, 
  uint nonce, 
  uint gasPrice, 
  uint gasLimit, 
  address gasToken, 
  OperationType operationType, 
  bytes32 extraHash, 
  bytes signatures) public
```


### Hash calculation
```js
    function calculateMessageHash(
      address from, 
      address to, 
      uint value, 
      bytes32 dataHash, 
      uint nonce, 
      uint gasPrice, 
      uint gasLimit, 
      address gasToken, 
      OperationType operationType, 
      bytes32 _extraHash) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(
          from, 
          to, 
          value, 
          dataHash, 
          nonce, 
          gasPrice, 
          gasLimit, 
          gasToken, 
          bytes4(0xc435c72c), 
          uint(operationType), 
          keccak256(bytes32(0x0))));
    }
```
To be added:
```
  callPrefix,
  operationType,
  extraHash
```

#### Interface 
```
contract ERC1077 {
    enum ExecutionType {MANAGEMENT, ACTION}
    enum OperationType {CALL, DELEGATECALL, CREATE}

    event ExecutedSigned(bytes32 signHash, uint nonce, bool success);

    function lastNonce() public view returns (uint nonce);

    function requiredSignatures(ExecutionType executionType) public view returns (uint);

    function executeSigned(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        uint gasLimit,
        address gasToken,
        OperationType operationType,
        bytes32 extraHash,
        bytes messageSignatures) public;

Optional:
    function getKey(bytes32 _key) public view returns(uint256 purpose, uint256 keyType, bytes32 key);
    function addKey(bytes32 _key, uint256 _purpose, uint256 _keyType) public returns (bool success);
 }
```

### REST API

POST /identity {managementKey, ensName}
POST /identity/execution {contractAddress, to, value, data, nonce, gasToken, gasPrice, gasLimit, signatures}

