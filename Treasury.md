# Smart Contract Description: Treasury Contract

The `Treasury` smart contract inherits from an abstract contract `BASE`. The `Treasury` contract provides functionality for managing the treasury's funds, including sending coins, locking/unlocking funds, and handling contract-based coin transfers.

## Contract Overview

### Events

- `CoinSent(address _to, uint256 _amount, uint256 _timestamp)`: Emitted when coins are sent.
- `GovernorIntervent(status indexed _s, uint256 _amount)`: Emitted when the governor intervenes (e.g., locks/unlocks funds).
- `NewCoinTransfer(address _hunter, uint256 _amount, uint256 _timestamp)`: Emitted when a new coin transfer occurs.

### Modifiers

- `onlyContracts`: Ensures that the function can only be called by specified contract addresses.

### Structs

#### `LastUnlocked`

- `uint256 amount`: Amount of funds to be unlocked.
- `uint256 avble`: Timestamp when funds become available for unlocking.
- `bool executed`: Indicates if the unlock has been executed.
- `bool pending`: Indicates if there is a pending unlock.

### State Variables

- `mapping(uint256 => LastUnlocked) public register`: Maps an index to the `LastUnlocked` struct.
- `uint256 idx`: Index for tracking unlocks.
- `uint256 public LOCKED_BALANCE`: The total amount of locked funds.
- `uint256 public MAX_UNLOCKABLE`: The maximum amount of funds that can be unlocked at once.
- `bool public inited`: Indicates if the contract has been initialized.

### Functions

#### System Functions

- `initContract(uint256 _balance, uint256 _amount) external onlyGovernor returns (bool)`: Initializes the contract with the specified balance and maximum unlockable amount. Reverts if the contract is already initialized.

#### Read Functions

- `AvailableBalance() public view returns (uint256)`: Returns the available balance (total balance minus locked balance).
- `TotalContractBalance() external view returns (uint256)`: Returns the total balance of the contract.

#### Governor Functions

- `sendCoin(address _addr, uint256 _amount) external onlyGovernor returns (bool)`: Sends the specified amount of coins to the given address. Reverts if there are insufficient funds or if the address is invalid.
- `lockFunds(uint256 _amount) external onlyGovernor returns (bool)`: Locks the specified amount of funds. Reverts if the amount exceeds the maximum lockable funds.
- `bookUnlockFunds(uint256 _amount) external onlyGovernor returns (bool)`: Books an unlock of the specified amount of funds. Reverts if there is already a pending unlock or if the amount exceeds the maximum unlockable funds.

#### Public Functions

- `unlockFunds() external returns (bool)`: Executes the pending unlock if the unlock time has passed. Reverts if there is no pending unlock or if the unlock time has not yet passed.

#### Contract Functions

- `contractCoinTransfer(address _addr, uint256 _amount) public onlyContracts returns (bool)`: Transfers the specified amount of coins to the given address. Reverts if the transfer fails.
