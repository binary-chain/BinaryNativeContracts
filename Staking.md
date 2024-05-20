# Smart Contract Description: Staking Contract

The `Staking` smart contract inherits from an abstract contract `BASE`. The `Staking` contract provides functionality for managing staking operations, including staking, unstaking, calculating rewards, and handling staking periods.

## Contract Overview

### State Variables

- `uint256 public TotalStakeAmount`: Total amount of staked coins in this contract.
- `uint256 public SeasonRewards`: Reference value for rewards in the current season.
- `uint256 _seasonRewards`: Setter for `SeasonRewards`.
- `uint256 public totSeasonStakableAmount`: Total amount that can be staked in the current season.
- `uint256 seasonStart`: Timestamp when the current season started.
- `uint256 public interest`: Reference value for annual percentage yield (APY).
- `uint256 _interest`: Setter for `interest`.
- `uint256 constant hundred = 1000000000`: 100% in basis points.
- `bool mintedAll`: Indicates if all rewards have been minted.
- `bool inited`: Indicates if the contract has been initialized.
- `uint256 public constant period = 60`: Duration of each season in seconds.
- `uint256 public maxStake`: Maximum stake per person in one season.
- `uint256 _maxStake`: Setter for `maxStake`.
- `uint256 public minStake`: Minimum stake per person in one season.
- `uint256 _minStake`: Setter for `minStake`.

### Structs

#### `Staker`

- `address addr`: Staker's address.
- `uint256 totStake`: Total amount staked by the staker.
- `uint256 nextAvble`: Index of the next available stake for withdrawal.
- `uint256[] stake`: Array of staked amounts.
- `uint256[] rewards`: Array of rewards for each stake.
- `uint256[] available`: Array of timestamps when each stake becomes available for withdrawal.

### Mappings

- `mapping(address => Staker) stakerByAddress`: Maps staker addresses to their staking data.

### Events

- `newStake(uint256 time, address indexed staker, uint256 amount)`: Emitted when a new stake is made.
- `newSeason(uint256 time, uint256 stakeAmount, uint256 reward)`: Emitted when a new staking season starts.

### Read Functions

- `getStaker(address _addr) external view returns(Staker memory)`: Returns the staking data for the given address.
- `getStakerInfo(address _addr) external view returns(address, uint256, uint256, uint256)`: Returns basic staking information for the given address.
- `getAddressStakesFromNextAvble(address _addr, uint256 _from) external view returns(uint256[] memory, uint256[] memory, uint256[] memory)`: Returns the stakes, rewards, and availability starting from the next available stake for the given address.
- `getAddressStakesFromIndex(address _addr, uint256 _from, uint256 _count) external view returns(uint256[] memory, uint256[] memory, uint256[] memory)`: Returns a range of stakes, rewards, and availability for the given address.
- `getNextStakerWithdraw(address _addr) external view returns(uint256, uint256, uint256)`: Returns the next available stake for withdrawal for the given address.

### Governor Functions

- `initContract(uint256 interest_, uint256 rewards_, uint256 minStake_, uint256 maxStake_) external onlyGovernor returns(bool)`: Initializes the contract with the specified parameters. Reverts if the contract is already initialized.
- `setSeason(uint256 rewards_, uint256 minStake_, uint256 maxStake_, uint256 interest_) external onlyGovernor returns(bool)`: Sets the parameters for the new season.
- `setSeasonRewards(uint256 amount) external onlyGovernor`: Sets the rewards for the new season.
- `setReward(uint256 interest_) external onlyGovernor`: Sets the interest rate for the new season.
- `setMinStake(uint256 amount) external onlyGovernor`: Sets the minimum stake amount.
- `setMaxStake(uint256 amount) external onlyGovernor`: Sets the maximum stake amount.

### Staker Functions

- `stake() external payable returns(bool)`: Allows a user to stake a specified amount. Reverts if the amount is incorrect or exceeds the maximum stake per account.
- `unstakeNextAvailable() external returns(bool)`: Allows a user to unstake the next available stake. Reverts if there are no available stakes or if it's not yet time to unstake.
- `unstakeAllAvailables() external returns(bool)`: Allows a user to unstake all available stakes. Reverts if there are no available stakes or if it's not yet time to unstake.

### Public Functions

- `startNewSeason() public returns(bool)`: Starts a new staking season if the current season has ended. Reverts if there are still rewards left to be minted.
