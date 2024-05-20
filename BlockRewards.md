# BlockRewards Contract

The `BlockRewards` contract extends this `BASE` contract functionality to manage block rewards, including initialization, reward retrieval, and setting new rewards within defined limits. The `onlyGovernor` modifier ensures that only authorized entities (admin or DAO) can modify the block reward.

## Contract Overview

### Events

- `BlockRewardChanged(uint256 _reward)`: Emitted when the block reward is changed.

### Constants

- `MIN_REWARD`: The minimum allowed reward per block (10^16 wei).
- `MAX_REWARD`: The maximum allowed reward per block (5 \* 10^17 wei).

### State Variables

- `uint256 reward`: The current block reward.
- `bool public inited`: Indicates whether the contract has been initialized.

### Functions

#### `initContract()`

- **Purpose**: Initializes the contract by setting the initial block reward.
- **Functionality**:
  - Sets the `reward` to 10^18 wei.
  - Sets `inited` to `true` to prevent re-initialization.
  - Reverts with `ContractAlreadyInitError` if the contract is already initialized.

#### `blockReward()`

- **Purpose**: Retrieves the current block reward.
- **Returns**: The current block reward.

#### `contractBalance()`

- **Purpose**: Retrieves the balance of the contract.
- **Returns**: The contract's balance in wei.

#### `setReward(uint256 _reward)`

- **Purpose**: Allows the governor to set a new block reward.
- **Functionality**:
  - Validates that the `_reward` is within the allowed range (`MIN_REWARD` to `MAX_REWARD`).
  - Sets the `reward` to the new value.
  - Emits `BlockRewardChanged` event with the new reward.
  - Reverts with `IncorrectRewardAmountError` if the reward is outside the allowed range.
