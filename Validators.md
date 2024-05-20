# Smart Contract Description: Validators Contract

This smart contract, `Validators`, inherits from an abstract contract `BASE`. The `Validators` contract manages the list of active validators.

## Contract Overview

### Events

- `NewValidator(string _str, address indexed _addr, uint256 _time)`: Emitted when a new validator is added.
- `StopValidator(string _str, address indexed _addr, uint256 _time)`: Emitted when a validator stops validating.
- `GovernorLockedValidator(string _str, address indexed _addr, uint256 _time)`: Emitted when a validator is locked by the governor.
- `GovernorUnlockedValidator(string _str, address indexed _addr, uint256 _time)`: Emitted when a validator is unlocked by the governor.
- `GovernorKickedValidator(address _addr)`: Emitted when a validator is kicked by the governor.

### Modifiers

- `onlyCoinbase`: Ensures that the function can only be called by the coinbase address.

### Structs

#### `Validator`

- `address addr`: Validator's address.
- `bool status`: Validator's status (active/inactive).
- `uint256 activeSince`: Timestamp when the validator became active.
- `uint256 stake`: Amount staked by the validator.
- `uint256 out`: Timestamp when the validator can unstake.
- `uint256 idx`: Index of the validator in the list.
- `bool violator`: Indicates if the validator is a violator.

### State Variables

- `bool init`: Indicates if the contract has been initialized.
- `uint256 stakeAmount`: Required stake amount to become a validator.
- `uint256 index`: Counter for validator indices.
- `address[] validatorsList`: List of active validators.
- `mapping(address => bool) activeValidator`: Maps addresses to their active status.
- `mapping(address => Validator) validatorByAddress`: Maps addresses to their validator data.
- `mapping(address => uint256) indexInValidatorsState`: Maps addresses to their index in `validatorsState`.
- `address[] validatorsState`: State of validators.

### Functions

#### System Functions

- `initContract() external returns (bool)`: Initializes the contract with predefined validators and sets the required stake amount. Reverts if the contract is already initialized.

#### Read Functions

- `getValidatorsLast() external view returns (Validator[] memory)`: Returns the last 25 validators.
- `get25ValidatorsFromIndex(uint256 _idx) external view returns (Validator[] memory)`: Returns 25 validators starting from a specific index.
- `readChangeInValidators() external view returns (address[] memory)`: Returns the state of validators.
- `checkValidator(address _addr) external view returns (bool)`: Checks if an address is an active validator.
- `getValitatorData(address _addr) external view returns (Validator memory)`: Retrieves validator data for a specific address.
- `getValidatorsList() external view returns (address[] memory)`: Returns the list of active validators.
- `getValidatorsCount() external view returns (uint256)`: Returns the total number of validators.
- `getStakeAmount() external view returns (uint256)`: Returns the required stake amount.
- `checkViolator(address _address) external view returns (bool)`: Checks if an address is marked as a violator.

#### Coinbase Functions

- `iVotedFor(address _address) external onlyCoinbase returns (bool)`: Handles voting for validators.

#### Validator Functions

- `stake() external payable returns (bool)`: Allows a user to become a validator by staking the required amount.
- `changeStatus() external returns (bool)`: Allows a validator to change their status.
- `unstake() external returns (bool)`: Allows a validator to unstake their amount if conditions are met.

#### Governor Functions

- `changeStakeAmount(uint256 _amount) external onlyGovernor returns (bool)`: Changes the required stake amount.
- `lockValidator(address _addr) external onlyGovernor returns (bool)`: Locks a validator.
- `unlockValidator(address _addr) external onlyGovernor returns (bool)`: Unlocks a validator.
- `kickValidator(address _addr) external onlyGovernor returns (bool)`: Kicks a validator and handles their stake appropriately.
