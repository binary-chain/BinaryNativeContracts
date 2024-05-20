# Smart Contract Description: LicenceSFC Contract

The `LicenceSFC` smart contract, inheriting from the abstract `BASE` contract, its manage license issuance, validation, deployment, and extension for smart contracts within the Binary Chain. It ensures that only licensed contracts are permitted to operate on the Binary Chain, maintaining a controlled and secure environment for smart contract execution.

## Contract Overview

### Events

- `NewValidatedContract(bytes32 _hash, string _name, address _owner)`: Emitted when a new contract is validated.
- `NewContractDeployed(uint256 _order, bytes32 _hash, address _contract, string _name, address _owner)`: Emitted when a new contract is deployed.
- `GovernanceDisabledLicence(address _contract)`: Emitted when a license is disabled by the governor.
- `GovernanceEnabledLicence(address _contract)`: Emitted when a license is enabled by the governor.

### Modifiers

- `onlyVotingContract`: Ensures that the function can only be called by the voting contract.

### Enums

#### `Action`

- `Absent`
- `Emitted`
- `Withdrawn`
- `Extended`
- `Expired`

#### `Status`

- `Absent`
- `Approved`
- `Deployed`

### Structs

#### `SFC`

- `bool status`: License status.
- `uint256 uuidLicenceSFC`: License UUID.
- `uint256 uuidAudit`: Audit UUID.
- `bytes32 contractHash`: Contract hash.
- `string contractName`: Contract name.
- `address contractAddr`: Contract address.
- `address deployerAddr`: Deployer address.
- `uint256 firstLicenceIssueDate`: First license issue date.
- `uint256 expiringDate`: License expiry date.
- `Data[] history`: License history.

#### `Data`

- `address authority`: Authority address.
- `uint256 date`: Date.
- `Action licenceAction`: License action.

### Mappings

- `mapping(bytes32 => Status) public contractStatus`: Maps contract hashes to their status.
- `mapping(bytes32 => string) public contractNameByHash`: Maps contract hashes to their names.
- `mapping(bytes32 => address) public deployerByHash`: Maps contract hashes to their deployer addresses.
- `mapping(string => address) public contractByName`: Maps contract names to their addresses.
- `mapping(address => SFC) licenceByAddress`: Maps contract addresses to their licenses.

### State Variables

- `uint256 public constant CHAIN_ID = 230`: Chain ID.
- `uint256 public constant YEAR = 31536000`: One year in seconds.
- `uint256 public licenceCost`: Cost of a license.
- `uint256 public salt`: Salt for contract deployment.
- `bool public inited`: Indicates if the contract has been initialized.

### Functions

#### Initialization Functions

- `initContract(uint256 _licenceCost) external onlyGovernor`: Initializes the contract with the specified license cost. Reverts if the contract is already initialized.

#### Read Functions

- `getLicenceByName(string memory _contractName) external view returns (SFC memory)`: Returns the license for the given contract name.
- `getLicenceByContractAddress(address _contractAddr) external view returns (SFC memory)`: Returns the license for the given contract address.
- `getContractNameByHash(bytes32 _contractHash) external view returns (string memory)`: Returns the contract name for the given hash.
- `getContractLicenceStatus(address _contractAddr) external view returns (bool)`: Returns the license status for the given contract address.
- `getContractName(address _contractAddr) external view returns (string memory)`: Returns the contract name for the given contract address.
- `isContractAbsent(bytes32 _contractHash) external view returns (bool)`: Checks if a contract is absent.
- `checkContractLicenseHistory(address _contractAddr) external view returns (Data[] memory)`: Returns the license history for the given contract address.

#### Contract Interaction Functions

- `validate(bytes32 _contractHash, string calldata _contractName, address _deployer) external onlyVotingContract returns (bool)`: Validates a contract.
- `deploy(bytes calldata _contractByteCode) external returns (address)`: Deploys a contract and returns its address.

#### Write Functions

- `extendLicence(address _contractAddr) external payable`: Extends a license. Reverts if the extension is not allowed or if the payment does not match the license cost.

#### Governor Functions

- `disableLicence(address _contractAddr) external onlyGovernor returns (bool)`: Disables a license. Reverts if the license does not exist or is already inactive.
- `enableLicence(address _contractAddr) external onlyGovernor returns (bool)`: Enables a license. Reverts if the license does not exist or is already active.

### Internal Functions

- `_deployAndGetAddr(bytes memory _contractByteCode, uint256 _salt) internal returns (address)`: Deploys a contract and returns its address.
- `_updateLicenceHistory(SFC storage _sfc, address _authority, Action _action) internal`: Updates the license history.
- `_setUsed(string memory _contractName) internal returns (bool)`: Sets a contract name to used status.
- `_getUUID(bytes32 _contractHash) internal view returns (uint256)`: Retrieves the UUID for a contract hash.
- `_checkIfLicenceExist(SFC storage s) internal view`: Checks if a license exists.
